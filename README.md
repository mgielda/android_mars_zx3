================
android_mars_zx3
================

Android default.xml file for Enclustra's Mars-ZX3 Zynq Module

Installation guide
==================

Step 0. Initial steps
---------------------

You will need to compile your Android-enabled kernel speparately. You can base your configuration on this file: https://github.com/antmicro/enclustra_zynq_linux/raw/master/config_enclustra_android

To compile 3.5 kernel from our repository:
<pre>
$ git clone https://github.com/antmicro/enclustra_zynq_linux.git
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


Step 4. Rootfs
--------------

If the compilation was successful, rootfs is located in out/target/product/mars_zx3/root.
Also two additional files have to be present on the rootfs - out/target/product/mars_zx3/system.img and out/target/product/mars_zx3/userdata.img.

<pre>
$ mkfs.ext2 /dev/sdx1   # replace sdx1 with a partition you want to use
$ mkdir /tmp/android_rootfs
$ mount /dev/sdx1 /tmp/android_rootfs
$ cp -r out/target/product/mars_zx3/root/* /tmp/android_rootfs/
$ cp out/target/product/mars_zx3/system.img /tmp/android_rootfs/
$ cp out/target/product/mars_zx3/userdata.img /tmp/android_rootfs
$ umount /tmp/android_rootfs
</pre>
