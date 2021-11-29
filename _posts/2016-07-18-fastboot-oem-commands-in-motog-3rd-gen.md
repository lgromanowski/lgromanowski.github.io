---
id: 84
title: Fastboot oem commands in MotoG (3rd ed.)
date: 2016-07-18T23:04:57+02:00
author: Łukasz Gromanowski
layout: post
guid: https://gromanowski.net/?p=84
permalink: /2016/07/18/fastboot-oem-commands-in-motog-3rd-gen/
categories:
  - android
tags:
  - bootloader
  - commands
  - fastboot
  - motog
  - motorola
  - oem
---
**DISCLAIMER: You&#8217;re using following information on your own risk. I&#8217;m not responsible if you brick your phone with any of those commands.**

### Commands available in normal fastboot mode

Here is the list of available commands in normal fastboot mode:

<pre class="lang:sh mark:5-10 decode:true">$ fastboot oem help
...
(bootloader) config...
(bootloader) partition...
(bootloader) fb_mode_set
(bootloader) fb_mode_clear
(bootloader) bp-tools-on
(bootloader) bp-tools-off
(bootloader) qcom-on
(bootloader) qcom-off
(bootloader) unlock
(bootloader) lock
(bootloader) get_unlock_data
(bootloader) cid_prov_req
(bootloader) read_sv
(bootloader) off-mode-charge
OKAY [  0.075s]
finished. total time: 0.075s</pre>

Meaning of _fb\_mode\_set, fb\_mode\_clear, bp-tools-on, bp-tools-off, qcom-on, qcom-off, cid\_prov\_req_ is unknown to me. Please share information in comments if you know what this commands are doing.

#### Fastboot oem config

<pre class="lang:sh decode:true">fastboot oem config [options]
  (none given)      list all supported utags
  list              list all configured utags
  &lt;name&gt; &lt;value&gt;    set a utag
  &lt;name&gt;            query a utag
  &lt;name&gt; ""
  unset &lt;name&gt;      unset a utag
  clear             unset all utags
  protect &lt;name&gt;    mark utag as read-only
  unprotect &lt;name&gt;  unmark utag as read-only
  help              show usage</pre>

#### fastboot oem partition

<pre class="lang:sh decode:true">$ fastboot oem partition help
...
(bootloader) usage: fastboot oem partition [options]
(bootloader) 
(bootloader) options:
(bootloader)   (none given)                     list all partitions
(bootloader)   &lt;name&gt;                           list a partition
(bootloader)   dump &lt;name&gt; [&lt;offset&gt; [size]]    dump a partition
(bootloader)   md5 &lt;name&gt; [&lt;offset&gt; [size]]     md5 of a partition
(bootloader)   sha256 &lt;name&gt; [&lt;offset&gt; [size]]  sha256 of a partition
(bootloader)   help                             show usage
(bootloader) 
OKAY [  0.068s]
finished. total time: 0.068s</pre>

#### fastboot oem read_sv

<pre class="lang:sh decode:true ">fastboot oem read_sv

Shows system(?) version</pre>

#### fastboot off\_mode\_charge

<pre class="lang:sh decode:true">fastboot off_mode_charge [mode]

usage:
    false, off, no, disable, 0: Don't use charger mode
    true, on, yes, enable, 1 : Default (charger)</pre>

### 

### Restricted commands

Below are commands which are &#8220;restricted&#8221; in normal fastboot mode (please let me know and share in comments if you know how to enable different fastboot modes where those commands can run):

#### fastboot oem usb_tune

<pre class="lang:sh decode:true">fastboot oem usb_tune [options]

options:
 &lt;empty&gt;            read current setting
 &lt;set&gt; &lt;value&gt;      tune USB using the &lt;value&gt;
                    &lt;value&gt; is a hex format int
 &lt;help&gt;             show usage

example: fastboot oem usb_tune set 0x020304</pre>

#### fastboot oem sp_test

<pre class="lang:sh decode:true">fastboot oem sp_test [options]
  add &lt;name&gt; &lt;size&gt;   add a new SP block
  remove &lt;name&gt;       remove a SP block
  help                show usage</pre>

#### fastboot oem led

<pre class="lang:sh decode:true ">fastboot oem led [options]
  on      turn LED off
  off     turn LED on
  blink   blink the led every half a second
  help    show usage</pre>

#### fastboot oem pmic

<pre class="lang:sh decode:true ">fastboot oem pmic [options]
  info                 show chip info
  rd &lt;reg&gt;             read from a register
  wr &lt;reg&gt; &lt;value&gt;     write to a register
  help                 show usage</pre>

#### fastboot oem regex

<pre class="lang:sh decode:true ">fastboot oem regex [options]
  match &lt;pattern&gt; &lt;string&gt;  match &lt;patten&gt; against &lt;string&gt;
  help                      show usage</pre>

#### fastboot oem wptest

<pre class="lang:sh decode:true ">fastboot oem wptest [options]
  enable         enable writeprotect
  disable        disable writeprotect
  test &lt;number&gt;  run a test
                 1 - display system image info
                 2 - enable write protect
                 3 - reset eMMC
                 4 - hash system image
                 5 - enable enhance erase
                 6 - check if system partition is writable
                 7 - enable hw_test (OTP) and print error code
          &lt;sector&gt; - check if specified sector is writable
  help           show usage</pre>

#### fastboot oem display

<pre class="lang:sh decode:true ">fastboot oem display [options]
  on            turn on display
  off           turn off display
  info          show display info
  help          show usage</pre>

#### fastboot oem backlight

<pre class="lang:sh decode:true">fastboot oem backlight [options]
  on                 turn on backlight
  off                turn off backlight
  &lt;number&gt;           set backlight level</pre>

#### fastboot oem logo

<pre class="lang:sh decode:true ">fastboot oem logo [options]
  show &lt;name&gt;    Show specified logo
  list           List all logos
  help           Show this usage
</pre>

#### fastboot oem ramdump

<pre class="lang:sh decode:true ">fastboot oem ramdump [options]
  (none given)        show status
  enable              enable RAM dumping
  disable             disable RAM dumping
  clear               clear existing RAM dumps
  pull [&lt;all&gt;|&lt;name&gt;] pull RAM dump files
  help                usage</pre>

&nbsp;