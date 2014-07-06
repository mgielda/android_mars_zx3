Introduction
============

This is a compilation and usage manual for the Android 4.1.2 operating system for `Enclustra's Mars ZX3 Zynq module <http://www.enclustra.com/en/products/system-on-chip-modules/mars-zx3/>`_.

Acknowledgements
----------------

We would like to thank Enclustra GmbH (http://enclustra.com) for co-sponsoring the port.

Version information
-------------------

.. csv-table::
   :header: Author,Content,Date,Version

   Peter Gielda,Draft version,07.07.2013,0.1
   Michael Gielda,Updated for Sphinx,08.07.2013,0.2
   Sebastian Kramer,Typos; required packages; improvements,08.07.2013,0.2.1
   Peter Gielda,Minor improvements,06.06.2014,0.2.2

Compiling the system
====================

The port was prepared using Gentoo and Debian Linux environments. The procedures described here should also work on other systems, but if you detect any errors or ommissions please e-mail us at `contact@antmicro.com <mailto:contact@antmicro.com>`_.

Prerequisites
-------------

Toolchain
~~~~~~~~~

The toolchain used to compile the port is Sourcery G++ Lite 2011.03-42, and can be obtained from `the Mentor Graphics website <https://sourcery.mentor.com/sgpp/lite/arm/portal/release1802>`_.

When this was written, Sourcery G++ Lite was directly available without the need for logging in using a direct link:

.. code-block:: bash

   wget https://sourcery.mentor.com/sgpp/lite/arm/portal/package8736/public/arm-none-eabi/arm-2011.03-42-arm-none-eabi.bin
   chmod +x arm-2011.03-42-arm-none-eabi.bin
   ./arm-2011.03-42-arm-none-eabi.bin

.. warning::

   The installer for the toolchain does not cope with dash as the system shell, however instructions how to get around that should be printed out when the installer is run. 

The toolchain should be decompressed and its :file:`bin` directory included in the PATH variable.
The proper availability of the toolchain can be checked by finding out if ``arm-none-eabi-gcc`` is available from the shell.

``repo``
~~~~~~~~

The ``repo`` tool, used to manipulate the Android git repositories, can be downloaded from Google's repositories and made available in your system as follows:

.. code-block:: bash

   mkdir ~/bin
   PATH=~/bin:$PATH
   curl https://raw.githubusercontent.com/pgielda/android_mars_zx3/master/tools/repo > ~/bin/repo
   chmod a+x ~/bin/repo

Java
~~~~

The Java JDK 1.6 is required for Android compilation. JDK 1.6 should be available in your system from the package manager, but it might require some effort to get it.

For example on a 64-bit system running Debian 7.0 (Wheezy) you need to add the non-free repository from Debian 6.0 (Squeeze) and get the JDK from there (this is best done as root):

.. code-block:: bash

   su
   echo "deb http://ftp.pl.debian.org/debian/ squeeze non-free" >> /etc/apt/sources.list
   aptitude update
   apt-get install sun-java6-jdk
   update-java-alternatives -s java-6-sun

Other packages
~~~~~~~~~~~~~~

A number of other dependencies is also required, but these should be available from package managers on most systems without a problem - e.g. on Debian 7.0 (Wheezy) you can get them by typing:

.. code-block:: bash

   sudo apt-get install lzop uboot-mkimage bison xsltproc zip flex
 
Building the Linux kernel
-------------------------

You will need to compile your Android-enabled kernel separately. You can base your configuration on the following file:

https://github.com/antmicro/enclustra_zynq_linux/raw/master/config_enclustra_android

To compile the 3.5 kernel from our repository:

.. code-block:: bash

   git clone https://github.com/antmicro/enclustra_zynq_linux.git
   cp config_enclustra_android .config
   CROSS_COMPILE="arm-none-eabi-" ARCH=arm make uImage -j5
   
Getting the Android sources
---------------------------

The sources are fetched using the ``repo`` tool:

.. code-block:: bash

   repo init -u git://github.com/antmicro/android_mars_zx3 -b master
   repo sync -f # use -f to ignore fetch errors

.. warning::

   Before starting this procedure, be aware that it may take a long time, especially if you are running on a slow Internet connection!

Building Android
----------------

Android can now be compiled using your Java installation. Be sure to supply the correct path to the JDK.

.. code-block:: bash

   source ./build/envsetup.sh
   export ANDROID_JAVA_HOME=/usr/lib/jvm/java-1.6.0-openjdk-amd64
   # valid on a 64-bit Debian 7.0 (Wheezy), replace with your path
   lunch mars_zx3-userdebug
   make

.. note::

   While compiling Android, some error messages in the form of ``find: 'src': No such file or directory`` might appear, but they do not in any way impede the compilation process and can be ignored.

Uploading the system on a USB stick
===================================

If the compilation was successful, the rootfs is located in ``out/target/product/mars_zx3/root``.
Two additional files also have to be present on the rootfs - ``out/target/product/mars_zx3/system.img`` and ``out/target/product/mars_zx3/userdata.img``.

The following procedure will produce a USB stick ready to run Android on the ZX3 module:

.. code-block:: bash

   mkfs.ext2 /dev/sdX1   # replace sdX1 with the partition you want to use
   mkdir /tmp/android_rootfs
   mount /dev/sdx1 /tmp/android_rootfs
   cp -r out/target/product/mars_zx3/root/* /tmp/android_rootfs/
   cp out/target/product/mars_zx3/system.img /tmp/android_rootfs/
   cp out/target/product/mars_zx3/userdata.img /tmp/android_rootfs
   umount /tmp/android_rootfs

