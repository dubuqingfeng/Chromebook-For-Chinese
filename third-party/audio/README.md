Crouton声卡驱动更新记录
==

##说明
由于众所周知的原因，Crouton脚本安装会卡到[声卡驱动](https://chromium.googlesource.com/chromiumos/third_party/adhd/+/master)那块。

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
( wget -O "$archive" "http://t.cn/R46YOzM" 2>&1 \
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
20160125:
```
commit	522d3d92f1780e126a22e7abb25c1f34cc6bb328
committer	chrome-bot <chrome-bot@chromium.org>	Wed Jan 20 22:25:28 2016
```
```
CRAS: rstream - close the shm fd

Don't leak the shm_fd.

BUG=chromium:575772
TEST=rstream_unittest.  Play and record audio from several streams, and
switch devices on Samus.

Change-Id: I12e14396025afb8d12a07ef871d909be46763abc
Signed-off-by: Dylan Reid <dgreid@chromium.org>
Reviewed-on: https://chromium-review.googlesource.com/322417
Reviewed-by: Ben Zhang <benzh@chromium.org>
```

20160104:

```
commit	418181cd234babbd3deafbd50307105de743ad5e
committer	chrome-bot <chrome-bot@chromium.org>	Wed Dec 30 23:29:06 2015
```
```
CRAS: alsa_card - Don't allow cards without a control

A recent change allowed for cards without a control.  That was stupid,
without a control there is no way to discover devices on the card.
Revert that part, but keep the part that allows for mixer-less cards.

BUG=chromium:572996
TEST=unittest and test with mixerless type-c dock

Signed-off-by: Dylan Reid <dgreid@chromium.org>
Change-Id: Ifa4e4bf0bb66efb965924c6a2f795c8183230814
Reviewed-on: https://chromium-review.googlesource.com/319976
```

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