Crouton声卡驱动更新记录
==

##说明
由于众所周知的原因，Crouton脚本安装会卡到声卡驱动那块。

因此修改脚本，以适应国情。

并定期维护驱动。

##使用方法

1.下载[Cronton](https://github.com/dnschneid/crouton)，打包下载，**Download Zip**

2.更改**targets/audio**文件:

第47行：

```
( wget -O "$archive" "$urlbase/$ADHD_HEAD.tar.gz" 2>&1 \
                                    || echo "Error fetching CRAS" ) | tee "$log"
```

改为：

```
( wget -O "$archive" "http://t.cn/R4t18A3" 2>&1 \
                                    || echo "Error fetching CRAS" ) | tee "$log"

```

3.直接运行```installer/main.sh```,或者make自己的crouton。

英文原版：

```
If you're modifying crouton, you'll probably want to clone or download the repo 
and then either run installer/main.sh directly, or use make to build your very 
own crouton. You can also download the latest release, cd into the Downloads 
folder, and run sh crouton -x to extract out the juicy scripts contained within, 
but you'll be missing build-time stuff like the Makefile.
```

##更新记录

20151227:

```
commitId:c14268822a6ddf28247bddffda383cbb15f052bb
committer:chrome-bot <chrome-bot@chromium.org>	Tue Dec 22 21:12:20 2015
```
```
CRAS: alsa_io - Include card name for USB nodes

When multiple USB audio devices are plugged, it's difficult to
figure out the input/ouput options from UI when they're all
named like 'Mic (USB)' or 'Speaker (USB)'.
This changed appends the card name to the existing display name
of USB nodes to avoid confusion.

BUG=chromium:535982
TEST=Plug Jabra speakerphone, verify the mic name on UI.

Change-Id: I4c5c16a459262f43d62bf7489d41270a334b2feb
Reviewed-on: https://chromium-review.googlesource.com/319311
Commit-Ready: Hsinyu Chao <hychao@chromium.org>
Tested-by: Hsinyu Chao <hychao@chromium.org>
Reviewed-by: Cheng-Yi Chiang <cychiang@chromium.org>
Reviewed-by: Dylan Reid <dgreid@chromium.org>
```