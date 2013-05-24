================
android_mars_zx3
================

Android default.xml file for Enclustra's Mars-ZX3 Zynq Module

Installation guide
==================

Step 0. Initial steps
---------------------

You will need to compile your Android-enabled kernel speparately. You can base your configuration on this file: https://github.com/pgielda/enclustra_zynq_linux/config_enclustra_android

To compile 3.5 kernel from our repository:
<pre>
$ git clone https://github.com/pgielda/enclustra_zynq_linux.git
$ cp config_enclustra_android .config
$ CROSS_COMPILE="arm-none-eabi-" ARCH=arm make uImage
</pre>

Step 1. Download repo tool
--------------------------

<pre>
$ mkdir ~/bin
$ PATH=~/bin:$PATH
$ curl https://dl-ssl.google.com/dl/googlesource/git-repo/repo > ~/bin/repo
$ chmod a+x ~/bin/repo
</pre>

Step 2. Init and sync repository
--------------------------------

<pre>
$ repo init -u git://github.com/antmicro/android_mars_zx3 -b master -m default.xml
$ repo sync
</pre>

Step 3. Compilation
-------------------

<pre>
$ source ./build/envsetup.sh
$ export ANDROID_JAVA_HOME=/opt/sun-jdk-1.6.0.31
$ lunch mars_zx3-userdebug
$ make
</pre>


