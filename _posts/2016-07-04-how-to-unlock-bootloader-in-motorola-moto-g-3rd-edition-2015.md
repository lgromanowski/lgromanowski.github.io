---
id: 6
title: How to unlock bootloader in Motorola Moto G (3rd edition, 2015)
date: 2016-07-04T23:25:33+02:00
author: Łukasz Gromanowski
layout: post
guid: http://gromanowski.net/?p=6
permalink: /2016/07/04/how-to-unlock-bootloader-in-motorola-moto-g-3rd-edition-2015/
categories:
  - android
tags:
  - android
  - bootloader
  - motog
  - motorola
  - osprey
  - unlock
---
Here you can find instructions how to unlock bootloader in Motorola Moto G in few simple steps (bear in mind that it makes your phone guaranty void). **Before you start please make a backup, because unlocking procedure will wipe all your data in similar manner as factory reset.**

<img loading="lazy" class="aligncenter size-full wp-image-26" src="https://gromanowski.net/wp-content/uploads/2016/07/motog3_mmpromo_product1_d.png" alt="Motorola MotoG 3rd gen." width="224" height="299" /> 

So, you have full backup of your phone, right? OK, let&#8217;s start. At first, you need to have two tools installed and working:  _adb (Android Debug Bridge)_ and _fastboot._ Both are available in Android SDK – you can install it with <a href="https://developer.android.com/studio/index.html" target="_blank">Android Studio</a>. To check if _adb_ is working correctly, you can connect your phone with USB cable (with USB debugging enabled on your phone) into your PC and run following command: _adb devices -l_ it should print output similar to the one below:

<pre class="lang:sh decode:true prettyprint lang-plain_text">$ adb devices -l
List of devices attached
* daemon not running. starting it now on port 5037 *
* daemon started successfully *
AA1111AAAA      device usb:3-1 product:cm_osprey model:MotoG3 device:osprey_umts</pre>

If it&#8217;s working then next step is rebooting phone into bootloader:

<pre class="lang:sh decode:true ">$ adb reboot bootloader</pre>

You should see &#8220;broken&#8221; android-robot on the screen with details about product, serial number, CPU etc. similar to the picture below, if this command succeed:

<img loading="lazy" class="wp-image-17 size-medium aligncenter" src="http://gromanowski.net/wp-content/uploads/2016/07/motog_bootloader-191x300.jpg" alt="Moto G bootloader screen" width="191" height="300" srcset="https://gromanowski.net/wp-content/uploads/2016/07/motog_bootloader-191x300.jpg 191w, https://gromanowski.net/wp-content/uploads/2016/07/motog_bootloader.jpg 370w" sizes="(max-width: 191px) 100vw, 191px" /> 

Next, please check if device is visible in fastboot:

<pre class="lang:sh decode:true prettyprint lang-plain_text">$ fastboot devices
AA1111AAAA      fastboot</pre>

If so, then we need to obtain bootloader unlock code which will be used on [Motorola Bootloader Unlock](https://motorola-global-portal.custhelp.com/app/standalone/bootloader/unlock-your-device-a/action/auth) webpage by issuing following command: `fastboot oem get_unlock_data`. Output should be similar to the one below:

<pre class="lang:sh decode:true prettyprint lang-javascript">$ fastboot oem get_unlock_data
...
(bootloader) 1A33240790131835#5F53621283961D
(bootloader) 174D46004D6F746F4733000000#3BAB
(bootloader) D30C36FE431061312875A449CC57D1D
(bootloader) AB234#B2D21F2700000000000000000
(bootloader) 0000000
OKAY [  0.239s]
finished. total time: 0.239s</pre>

And the last step is to go to mentioned earlier [Motorola Bootloader Unlock](https://motorola-global-portal.custhelp.com/app/standalone/bootloader/unlock-your-device-a/action/auth) webpage and follow the instructions there to receive e-mail with unlock code.

Use the code from e-mail to unlock bootloader:

<pre class="lang:sh decode:true prettyprint lang-plain_text ">$ fastboot oem unlock UNIQUE_KEY</pre>

If everything goes OK, then phone should reboot (sometimes phone must be rebooted manually – you can do this by using <kbd>Vol-Up</kbd> / <kbd>Vol-Down</kbd> buttons to select proper option in bootloader&#8217;s menu and pressing <kbd>Power</kbd> button).

Yikes! Your phone is unlocked – you can install custom ROM now (ie. <del><a href="http://wiki.cyanogenmod.org/w/Main_Page">Cyanogen MOD</a></del> <a href="http://wiki.lineageos.org/osprey_install.html" target="_blank">LineageOS</a> )

Till next Time!