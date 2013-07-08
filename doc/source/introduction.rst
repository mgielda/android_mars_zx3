Introduction
============

This is a compilation and usage manual for the port of the Android 4.0 operating system for Enclustra's Mars ZX3 Zynq module.

We would like to thank Enclustra GmbH (http://enclustra.com) for co-sponsoring the port.

Version information
-------------------

.. csv-table::
   :header: Author,Content,Date,Version

   Peter Gielda,Draft version,07.07.2013,0.1
   Michael Gielda,Updated for Sphinx,08.07.2013,0.2

Compiling the system
====================

The port was prepared using Gentoo and Debian Linux environments. The procedures described here should also work on other systems, but if you detect any errors or ommissions please e-mail us at `contact@antmicro.com <mailto:contact@antmicro.com>`_.

Prerequisites
-------------

``repo``
~~~~~~~~

The ``repo`` tool, used to manipulate the Android git repositories, can be downloaded from Google's repositories and made available in your system as follows:

.. code-block:: bash

   $ mkdir ~/bin
   $ PATH=~/bin:$PATH
   $ curl https://dl-ssl.google.com/dl/googlesource/git-repo/repo > ~/bin/repo
   $ chmod a+x ~/bin/repo

``lunch``
~~~~~~~~~

Lunch should be available as a system package. On Debian, it can be installed by typing:

.. code-block:: bash

   sudo apt-get install python-lunch

Java
~~~~

The Java JDK 1.6 is required for Android compilation. JDK 1.6 should be available in your system from the package manager, for example on Debian you can install it by typing:

.. code-block:: bash

   sudo apt-get install openjdk-6-jdk

Building the Linux kernel
-------------------------

You will need to compile your Android-enabled kernel speparately. You can base your configuration on the following file:

https://github.com/antmicro/enclustra_zynq_linux/raw/master/config_enclustra_android

To compile the 3.5 kernel from our repository:

.. code-block:: bash

   $ git clone https://github.com/antmicro/enclustra_zynq_linux.git
   $ cp config_enclustra_android .config
   $ CROSS_COMPILE="arm-none-eabi-" ARCH=arm make uImage

Getting the Android sources
---------------------------

The sources are fetched using the ``repo`` tool:

.. code-block:: bash

   $ repo init -u git://github.com/antmicro/android_mars_zx3 -b master \
                                                        -m default.xml
   $ repo sync

.. warning::

   Before starting this procedure, be aware that it may take a long time, especially if you are running on a slow Internet connection!

Building Android
----------------

Android can now be compiled using your Java installation. Be sure to supply the correct path to the JDK.

.. code-block:: bash

   $ source ./build/envsetup.sh
   $ export ANDROID_JAVA_HOME=/usr/lib/jvm/java-6-openjdk-amd64
     # valid on Debian 7.0 (Wheezy), replace with your path
   $ lunch mars_zx3-userdebug
   $ make

Uploading the system on a USB stick
===================================

If the compilation was successful, the rootfs is located in ``out/target/product/mars_zx3/root``.
Two additional files also have to be present on the rootfs - ``out/target/product/mars_zx3/system.img`` and ``out/target/product/mars_zx3/userdata.img``.

The following procedure will produce a USB stick ready to run Android on the ZX3 module:

.. code-block:: bash

   $ mkfs.ext2 /dev/sdX1   # replace sdX1 with the partition you want to use
   $ mkdir /tmp/android_rootfs
   $ mount /dev/sdx1 /tmp/android_rootfs
   $ cp -r out/target/product/mars_zx3/root/* /tmp/android_rootfs/
   $ cp out/target/product/mars_zx3/system.img /tmp/android_rootfs/
   $ cp out/target/product/mars_zx3/userdata.img /tmp/android_rootfs
   $ umount /tmp/android_rootfs




