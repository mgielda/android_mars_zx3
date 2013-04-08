================
android_mars_zx3
================

Android default.xml file for Enclustra's Mars-ZX3 Zynq Module

Installation guide
==================


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


