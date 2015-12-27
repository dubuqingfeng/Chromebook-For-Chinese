# 替换 Chrome OS 为自由的 GNU/Linux

## 准备工作

了解你的 Chromebook [具体型号](https://wiki.archlinux.org/index.php/Chrome_OS_devices/Chromebook)，尤其是它的[代号](https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices) (codename) 。

移除你的设备的写保护螺丝，可以参考[这里](https://wiki.archlinux.org/index.php/Chrome_OS_devices#Flashing_a_custom_firmware)。每个设备的写保护螺丝位置不相同，你可以咨询你设备的制造商，不过通常你需要首先拆除设备的后盖。除去该螺丝可以使你自由地刷写固件 (firmware) 。在做完全部操作之后你既可以选择装回这颗螺丝，也可以选择不装回。

打开[开发者模式](http://www.howtogeek.com/210817/how-to-enable-developer-mode-on-your-chromebook/)。

一个至少 2GB 的 USB 存储设备，最好是 U 盘。

自由地访问互联网。这虽然在多数情况下不是必备条件，但这会让你在遇到问题时解决问题变得容易。

**[重要]** 具备一定的 Linux 常识，并做好弄坏你的设备的准备，因为笔者不保证下述过程可以在你的设备上成功重现，而且笔者不承担任何责任。

## 实验环境

```
Dell Chromebook 11 (Wolf)  
Celeron 2955U  
4GB RAM  
16GB emmc hard drive  
Fedora Workstation 23  
```

## 刷写第三方定制的 Coreboot / Seabios

首先按 Ctrl + Alt + T 进入模拟终端（其实看上去就是一个 Chrome 标签页），键入 `shell` 进入 shell 。

社区开发者 John Lewis 创造了一个[通用脚本](https://johnlewis.ie/custom-chromebook-firmware/rom-download/)，你可以选择使用他的脚本刷写第三方固件。整个过程是完全自动的，也就是说，你在键入下述命令、选择恰当的选项并点击回车之后就可以小憩了：

```bash
cd; rm -f flash_chromebook_rom.sh; curl -L -O https://johnlewis.ie/flash_chromebook_rom.sh; sudo -E bash flash_chromebook_rom.sh
```

虽然这个脚本很简单，通常情况下也是安全的，但是还是建议你仔细阅读上述网页中的提示。你也可以选择阅读他的脚本，反正不长，读完大概需要 5 分钟左右。

在字符选项界面你需要根据你的设备型号选择刷写固件的模式，你设备支持的模式需要查阅上述网页中的表格。我选择的是 full rom 。

## 重新启动电脑

如果没有错误提示，你就可以重新启动计算机了。

你会看一个黑色的界面，光标不断跳动，会有 Booting from Hard Disk 的字样，就像这样：

![](http://i.imgur.com/YntXS6y.jpg =500x)

这是因为 Seabios 尝试从硬盘引导，而它找不到可以启动的操作系统。不要慌张，可以按电源键关机。

## 制作可启动的安装 U 盘

具体参阅你使用的发行版的主页，比如 Ubuntu 的可以看[这里](https://help.ubuntu.com/community/Installation/FromUSBStick)。总之是大同小异。

笔者很有经验所以选择直接 dd 的方式：

```
sudo dd if=Fedora-Live-Workstation-x86_64-23-10.iso of=/dev/sdc bs=8M && sync
```

## 从 USB 引导

这里有两个小技巧：

首先是你开机之后可能看不到 USB 启动选项，这时如果有 Jeltka 这个选项的话，可以先进入 Jeltka ，用 root 登录，然后 poweroff ，这时你应该就能看到 USB 启动了。

其次是你的键盘可能没有响应，这会导致你在 Seabios 里按 ESC 却看不到启动设备选单。解决方法很简单，外接 USB 键盘即可。

然后你就可以像别的计算机一样安装你心爱的 GNU/Linux 发行版了。

## 解决休眠唤醒

### 方法一：通过 unbind 音频设备和拉黑 ehci-pci

首先增加一个新的文件 `/etc/tmpfiles.d/cros-acpi-wakeup.conf` 并修改内容为： 

```
# /etc/tmpfiles.d/cros-acpi-wakeup.conf
w /proc/acpi/wakeup - - - - EHCI
w /proc/acpi/wakeup - - - - HDEF
w /proc/acpi/wakeup - - - - XHCI
w /proc/acpi/wakeup - - - - LID0
w /proc/acpi/wakeup - - - - TPAD
w /proc/acpi/wakeup - - - - TSCR
```

然后再新建一个新的 systemd 脚本 `/usr/lib/systemd/system-sleep/cros-sound-suspend.sh`：

```
# /usr/lib/systemd/system-sleep/cros-sound-suspend.sh
#!/bin/bash

case $1/$2 in
  pre/*)
    # Unbind ehci for preventing error 
    echo -n "0000:00:1d.0" | tee /sys/bus/pci/drivers/ehci-pci/unbind
    # Unbind snd_hda_intel for sound
    echo -n "0000:00:1b.0" | tee /sys/bus/pci/drivers/snd_hda_intel/unbind
    echo -n "0000:00:03.0" | tee /sys/bus/pci/drivers/snd_hda_intel/unbind
    ;;
  post/*)
    # Bind ehci for preventing error 
    echo -n "0000:00:1d.0" | tee /sys/bus/pci/drivers/ehci-pci/bind
    # bind snd_hda_intel for sound
    echo -n "0000:00:1b.0" | tee /sys/bus/pci/drivers/snd_hda_intel/bind
    echo -n "0000:00:03.0" | tee /sys/bus/pci/drivers/snd_hda_intel/bind
    ;;
esac
```

然后在 `/etc/default/grub` 里增加一句：

```
GRUB_CMDLINE_LINUX_DEFAULT="modprobe.blacklist=ehci_pci"
```

然后 rebuild `grub.cfg`：

```
grub2-mkconfig -o /boot/grub/grub2.cfg
```

提醒一下，上面这句是发行版相关的，你用的 Ubuntu 的话估计命令会不太一样。

最后重启：

```
sudo reboot
```

### 方法二：通过刷写 CoolStar 的 BIOS 

想办法从 CoolStar 那里要一个 build 好的 BIOS ，然后直接 `flashrom -w <filename>` 。他的 BIOS 调的比较细致，具体调了什么你可以在 G+ 上亲自问他。

## 参考资料

[1] Arch Wiki, [https://wiki.archlinux.org/index.php/Chrome\_OS\_devices](https://wiki.archlinux.org/index.php/Chrome_OS_devices)  
[2] A Post on Arch Community, [https://bbs.archlinux.org/viewtopic.php?pid=1364521#p1364521](https://bbs.archlinux.org/viewtopic.php?pid=1364521#p1364521)  
[3] A Blog Post, [http://blog.sergiodj.net/post/2014-09-26-fedora-on-acer-c720p/](http://blog.sergiodj.net/post/2014-09-26-fedora-on-acer-c720p/)  

