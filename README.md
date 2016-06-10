# 在中国如何使用chromebook
## 激活服务

chromebook到手以后，使用Chrome os的时候需要激活，登录好谷歌帐号，以后就可以直接登录了。

激活的时候正常需要翻墙，可以使用fqrouter2等进行手机USB分享，不过fqrouter是root后的android设备使用起来更佳，应该也可以局域网中使用shadowsocks代理服务，chromebook连接同一个局域网，设置代理激活。

## 替换原厂固件和操作系统

适用于：

1. 具备一定的 GNU/Linux 知识，希望在 Chromebook 上获得完整的 GNU/Linux 作业环境的用户;

2. 因为种种原因希望替换 Google 提供的网络服务的用户。

请点击 [为 Chromebook 刷写第三方操作系统](linux-on-seabios-and-coreboot.md) 。

## 在 Chromebook 上安装 Ubuntu

### 安装方法

#### Chrubuntu

参见 http://chromeos-cr48.blogspot.com/2013/10/chrubuntu-for-new-chromebooks-now-with.html

简单记录如下：

**注意**：以下操作将抹除 Chrome OS 的用户数据，请做好重要数据的备份

1. 进入开发者模式
2. 不要登录，确保联网，然后按下 CTRL + ALT + =>(F2) 进入 TTY
3. 登录，用户名 chronos，密码不填
4. 输入命令：

```
	curl -L -O http://t.cn/RA3xv2I; 
	sudo bash RA3xv2I
```

5. 根据提示选择分区大小，将硬盘重新分区，之后会自动重启一次
6. 重新分区后，需要再执行一次上面的命令。注意默认会安装最新版的 Ubuntu，如需安装 LTS 版本请在后一句最后加上 ` -u lts`，更多选项请加上 ` -h` 查看
7. 接下来根据提示进行即可，安装完后默认的主机名为 chrubuntu，用户名跟密码都为 user

##### 修改说明

- 文件 chrubuntu.sh 来自于 http://goo.gl/9sgchs ，作了如下改动：
  1. 修改官方源为国内源，将 cdimage 的源改成 USTC，更新源换成 THU
  2. 去掉了安装 Google Chrome 的部分
  3. 最新15.04版本内核为3.19，应该不需要替换内核补丁。最后是用U盘装好的。
  4. [Desktop install](https://github.com/karlssonjohan/ubuntu-on-chromebook)
- 文件 cros-haswell-modules.sh 来自于 [短链接goo.gl](http://goo.gl/kz917j) ，未作改动

U盘引导安装：

用到的工具：Win32DiskImager

[关于使用后的U盘恢复](http://blog.csdn.net/u011538446/article/details/11590825)

#### Cronton

[Chromium OS Universal Chroot Environment](https://github.com/dnschneid/crouton)

下载官方Cronton:

```wget https://goo.gl/fd3zc```

[Cronton安装需要的声卡驱动](https://raw.githubusercontent.com/dubuqingfeng/Chromebook-For-Chinese/master/third-party/audio/latest.tar.gz)

##### 声卡驱动修改说明：
1.下载[Cronton](https://github.com/dnschneid/crouton)，打包下载，**Download Zip**

2.更改**targets/audio**文件:

第47行：

```
( wget -O "$archive" "$urlbase/$ADHD_HEAD.tar.gz" 2>&1 \
                                    || echo "Error fetching CRAS" ) | tee "$log"
```

改为：

```
( wget -O "$archive" "http://t.cn/R46YOzM" 2>&1 \
                                    || echo "Error fetching CRAS" ) | tee "$log"

```

3.直接运行```installer/main.sh```,或者make自己的crouton。

#### elementary OS

[elementary OS installation script for Chromebooks](https://github.com/Setsuna666/elementaryos-chromebook)

## 运行安卓程序

## 触摸板内核问题
如果是刚刚装好系统，还需要先安装 sudo gcc 等软件包。

```
$sudo pacman -S wget sudo patch make gcc
wget http://t.cn/RA3CM2t
bash RA3CM2t \$mykern
rm RA3CM2t
```

## 代理服务

+ shadowsocks-chromeapp: [Chromebook/ChromeOS安装Shadowsocks扩展教程](https://www.dogfight360.com/blog/?p=250)
+ fqrouter2
+ VPN
+ SSH代理

## 链接
