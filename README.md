# 在 Chromebook 上安装 ChrUbuntu

## 安装方法

参见 http://chromeos-cr48.blogspot.com/2013/10/chrubuntu-for-new-chromebooks-now-with.html

简单记录如下：

**注意**：以下操作将抹除 Chrome OS 的用户数据，请做好重要数据的备份

1. 进入开发者模式
2. 不要登录，确保联网，然后按下 CTRL + ALT + =>(F2) 进入 TTY
3. 登录，用户名 chronos，密码不填
4. 输入命令：`curl -L -O http://t.cn/RZsywEj; sudo bash RZsywEj`
5. 根据提示选择分区大小，将硬盘重新分区，之后会自动重启一次
6. 重新分区后，需要再执行一次上面的命令。注意默认会安装最新版的 Ubuntu，如需安装 LTS 版本请在后一句最后加上 ` -u lts`，更多选项请加上 ` -h` 查看
7. 接下来根据提示进行即可，安装完后默认的主机名为 chrubuntu，用户名跟密码都为 user

## 修改说明

- 文件 chrubuntu.sh 来自于 http://goo.gl/9sgchs ，作了如下改动：
  1. 修改官方源为国内源，将 cdimage 的源改成 USTC ，更新源改成 THU
  2. 去掉了安装 Google Chrome 的部分
- 文件 cros-haswell-modules.sh 来自于 http://goo.gl/kz917j ，未作改动
