---
id: 29
title: Get rid of bootloader unlocked warning
date: 2016-07-10T08:52:38+02:00
author: Łukasz Gromanowski
layout: post
guid: https://gromanowski.net/?p=29
permalink: /2016/07/10/get-rid-of-bootloader-unlocked-warning/
categories:
  - android
tags:
  - adb
  - android
  - bootloader
  - motog
  - motorola
  - osprey
---
In [previous article](https://gromanowski.net/2016/07/04/how-to-unlock-bootloader-in-motorola-moto-g-3rd-edition-2015/) we unlocked MotoG&#8217;s bootloader which resulted in the appearance of nasty &#8220;bootloader unlocked&#8221; warning on the screen.

<img loading="lazy" class="aligncenter size-full wp-image-42" src="https://gromanowski.net/wp-content/uploads/2016/07/logo_unlocked.png" alt="Warning about unlocked bootloader" width="540" height="438" srcset="https://gromanowski.net/wp-content/uploads/2016/07/logo_unlocked.png 540w, https://gromanowski.net/wp-content/uploads/2016/07/logo_unlocked-300x243.png 300w" sizes="(max-width: 540px) 100vw, 540px" /> 

To get rid of it we have to dump logo partition, edit it&#8217;s contents and flash it back to the MotoG.

### Dumping logo partition

Let&#8217;s start – enable root access via adb in Settings -> Developer options and connect to your phone via _adb shell_. We need to find which device is mounted as _logo_ partition, so we have to run: _ls -al /dev/block/platform/msm_sdcc.1/by-name_ to enumerate device partitions with aliases:

<pre class="decode-attributes:false lang:sh mark:23 decode:true prettyprint lang-sh">root@osprey:/# ls -al /dev/block/platform/msm_sdcc.1/by-name                                                                                                                                                                              
total 0
drwxr-xr-x 2 root root 880 1970-04-21 20:46 .
drwxr-xr-x 4 root root 960 1970-04-21 20:46 ..
lrwxrwxrwx 1 root root  20 1970-04-21 20:46 DDR -&gt; /dev/block/mmcblk0p3
lrwxrwxrwx 1 root root  20 1970-04-21 20:46 aboot -&gt; /dev/block/mmcblk0p4
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 abootBackup -&gt; /dev/block/mmcblk0p11
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 boot -&gt; /dev/block/mmcblk0p31
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 cache -&gt; /dev/block/mmcblk0p40
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 carrier -&gt; /dev/block/mmcblk0p38
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 cid -&gt; /dev/block/mmcblk0p25
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 clogo -&gt; /dev/block/mmcblk0p28
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 customize -&gt; /dev/block/mmcblk0p39
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 dhob -&gt; /dev/block/mmcblk0p22
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 frp -&gt; /dev/block/mmcblk0p17
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 fsc -&gt; /dev/block/mmcblk0p24
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 fsg -&gt; /dev/block/mmcblk0p23
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 hob -&gt; /dev/block/mmcblk0p21
lrwxrwxrwx 1 root root  20 1970-04-21 20:46 hyp -&gt; /dev/block/mmcblk0p7
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 hypBackup -&gt; /dev/block/mmcblk0p14
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 keystore -&gt; /dev/block/mmcblk0p36
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 kpan -&gt; /dev/block/mmcblk0p30
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 logo -&gt; /dev/block/mmcblk0p27
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 logs -&gt; /dev/block/mmcblk0p16
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 metadata -&gt; /dev/block/mmcblk0p26
lrwxrwxrwx 1 root root  20 1970-04-21 20:46 misc -&gt; /dev/block/mmcblk0p9
lrwxrwxrwx 1 root root  20 1970-04-21 20:46 modem -&gt; /dev/block/mmcblk0p1
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 modemst1 -&gt; /dev/block/mmcblk0p19
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 modemst2 -&gt; /dev/block/mmcblk0p20
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 oem -&gt; /dev/block/mmcblk0p37
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 padA -&gt; /dev/block/mmcblk0p10
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 padB -&gt; /dev/block/mmcblk0p18
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 padC -&gt; /dev/block/mmcblk0p34
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 persist -&gt; /dev/block/mmcblk0p29
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 recovery -&gt; /dev/block/mmcblk0p32
lrwxrwxrwx 1 root root  20 1970-04-21 20:46 rpm -&gt; /dev/block/mmcblk0p5
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 rpmBackup -&gt; /dev/block/mmcblk0p12
lrwxrwxrwx 1 root root  20 1970-04-21 20:46 sbl1 -&gt; /dev/block/mmcblk0p2
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 sp -&gt; /dev/block/mmcblk0p35
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 ssd -&gt; /dev/block/mmcblk0p33
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 system -&gt; /dev/block/mmcblk0p41
lrwxrwxrwx 1 root root  20 1970-04-21 20:46 tz -&gt; /dev/block/mmcblk0p6
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 tzBackup -&gt; /dev/block/mmcblk0p13
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 userdata -&gt; /dev/block/mmcblk0p42
lrwxrwxrwx 1 root root  20 1970-04-21 20:46 utags -&gt; /dev/block/mmcblk0p8
lrwxrwxrwx 1 root root  21 1970-04-21 20:46 utagsBackup -&gt; /dev/block/mmcblk0p15</pre>

Great, we found logo partition (line 23), so we can copy it to _/sdcard_. The easiest way is to use _dd_ utility: _dd if=/dev/block/mmcblk0p27 of=/sdcard/logo-copy.bin_

<pre class="lang:sh decode:true">root@osprey:/# dd if=/dev/block/mmcblk0p27 of=/sdcard/logo-copy.bin
8192+0 records in
8192+0 records out
4194304 bytes transferred in 1.179 secs (3557509 bytes/sec)</pre>

After that exit from adb shell and pull file from device by running: _adb pull /sdcard/logo-copy.bin ~/logo-copy.bin_ 

<pre class="lang:sh decode:true">$adb pull /sdcard/logo-copy.bin ~/logo-copy.bin
3638 KB/s (4194304 bytes in 1.125s)</pre>

and voilà. We can edit it and disable this warning.

### Editing dumped logo partition contents

We need to have hexeditor installed – personally I prefer [Okteta](https://www.kde.org/applications/utilities/okteta/), so all examples here are based on it. Depending on your Linux distribution you can get it by running:

Ubuntu: _sudo apt-get install okteta_

Fedora: _sudo yum install okteta_

Arch: _sudo pacman -S okteta_

OK. Let&#8217;s open our logo.bin in hex editor. As you can see on a picture below, there is some file header with a few picture items listed (file structure will be described in next article). We have to change value of six bytes: &#8220;**12 08 00 66 5E 02**&#8221; placed at offset **0000:0066** (second item marked with green) with a value &#8220;**04 00 00 56 D1 07**&#8221; – the same value which is visible at offset **0000:0026** (first item marked with green).<figure id="attachment_77" aria-describedby="caption-attachment-77" style="width: 901px" class="wp-caption aligncenter">

<img loading="lazy" class="wp-image-77 size-full" src="https://gromanowski.net/wp-content/uploads/2016/07/logo_bin_in_okteta-1.png" alt="Logo.bin in Okteta" width="901" height="443" srcset="https://gromanowski.net/wp-content/uploads/2016/07/logo_bin_in_okteta-1.png 901w, https://gromanowski.net/wp-content/uploads/2016/07/logo_bin_in_okteta-1-300x148.png 300w, https://gromanowski.net/wp-content/uploads/2016/07/logo_bin_in_okteta-1-768x378.png 768w" sizes="(max-width: 901px) 100vw, 901px" /> <figcaption id="caption-attachment-77" class="wp-caption-text">Logo.bin in Okteta</figcaption></figure> 

After change both items should have the same value: &#8220;**04 00 **00 56 D1 07****&#8221; – it&#8217;s a position and length of picture data used by specific image. So, right now _logo_unlocked_ picture will be using _logo_boot_ picture data and we&#8217;ll see nice blue &#8220;M&#8221; logo after flashing this file and restarting phone. Save the file with _logo-fixed.bin_ file name, it will be used in next step.

### Flashing fixed partition contents to the phone

In this step we&#8217;re going to flash _logo-fixed.bin_ file on phone, we have to restart phone to the bootloader:

<pre class="lang:sh decode:true ">$adb restart bootloader</pre>

and next use fastboot to flash _logo-fixed.bin_ file into _logo_ partition:

<pre class="lang:sh decode:true">$fastboot flash logo logo-fixed.bin</pre>

Restart phone:

<pre class="lang:sh decode:true ">$fastboot restart</pre>

and we&#8217;re done. We have nice &#8220;M&#8221; logo :)

Till next time!