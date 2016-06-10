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
20160610:
```
commit	828868cb145790177903b68e229a397376e60c0c
committer	chrome-bot <chrome-bot@chromium.org>	Thu Jun 09 02:17:42 2016
```
```
CRAS: Allow ALSA period_frames to be specified.

Some devices need to setup the period_frames before audio is written
to the mmap in order to configure down-stream devices such as a DSP
that pulls the data from the memory map based on a timer.

This change allows the period_frames to be specified per io-device,
by adding a new UCM configuration parameter. Configuration of the
period frames is done within the normal ALSA hwparam setup.

BUG=None
TEST=When MinPeriodFrames is not used, nothing changes.
When MinPeriodFrames is defined, the value is modified within
the device's ALSA hwparams.
Add unit tests to cover the new configuration.
```
20160330:

```
commit	bb0e945c6117de3725f4c1b92e277d80173a99fc
committer	chrome-bot <chrome-bot@chromium.org>	Wed Mar 30 09:15:07 2016
```

```
UCM config: glados and chell: Don't apply speaker EQ/DRC to headphones

Set the OutputDspName to empty for the Headphone jack, so that
get_active_dsp_name doesn't return the default "speaker_eq" dsp
for the headphone node.

BUG=chrome-os-partner:51560
TEST=On Chell set some extreme speaker EQ values in dsp.ini, listen
to headphone playback and confirm the speaker EQ/DRC is not applied.
```

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
```