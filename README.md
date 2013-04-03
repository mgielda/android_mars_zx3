android_mars-zx3
================

Android default.xml file for Enclustra's Mars-ZX3 Zynq Module

Installation guide
==================

<pre>
repo init -u git://github.com/pgielda/android_mars-zx3 -b master -m default.xml
repo sync
source ./build/envsetup.sh
lunch mars-zx3-userdebug
make
</pre>

