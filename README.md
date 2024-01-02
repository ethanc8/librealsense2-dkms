# librealsense2-dkms

This branch contains Intel's DKMS modules for kernels 5.15, 5.19, and 6.2 on Ubuntu 20.04 and Ubuntu 22.04.

The source was obtained through the following:

```bash
wget https://librealsense.intel.com/Debian/apt-repo/pool/jammy/main/librealsense2-dkms_1.3.24-0ubuntu1_all.deb
dpkg-deb -R ./librealsense2-dkms_1.3.24-0ubuntu1_all.deb librealsense2-dkms_1.3.24
```

You can build a Debian package from this source code using

```bash
git clone https://github.com/ethanc8/librealsense2-dkms --branch raspbian-12
rm librealsense2-dkms/README.md
fakeroot dpkg-deb --build librealsense2-dkms librealsense2-dkms_1.3.24-1+6.1.63+rpi3.deb
```

## Using the files from the Raspbian kernel

```bash
mkdir rpi-6.1 && cd rpi-6.1
# Get the URLs from http://archive.raspberrypi.org/debian/pool/main/l/linux/
wget http://archive.raspberrypi.org/debian/pool/main/l/linux/linux_6.1.63.orig.tar.xz
wget http://archive.raspberrypi.org/debian/pool/main/l/linux/linux_6.1.63-1+rpt1.dsc
wget http://archive.raspberrypi.org/debian/pool/main/l/linux/linux_6.1.63-1+rpt1.debian.tar.xz
dpkg-source -x ./linux_6.1.63-1+rpt1.dsc
# The kernel source is in rpi-6.1/linux-6.1.63/
dkmsdir=librealsense2-dkms/usr/src/librealsense2-dkms-1.3.24/6.1.0
sourcedir=rpi-6.1/linux-6.1.63
rm -r $dkmsdir
mkdir -p $dkmsdir/drivers/media
mkdir -p $dkmsdir/drivers/media/usb
mkdir -p $dkmsdir/include-overrides
mkdir -p $dkmsdir/include-overrides/uapi/linux
cp -r $sourcedir/drivers/media/usb/uvc $dkmsdir/drivers/media/usb/uvc
cp -r $sourcedir/drivers/media/v4l2-core $dkmsdir/drivers/media/v4l2-core
cp -r $sourcedir/include/asm-generic $dkmsdir/include-overrides/asm-generic
cp -r $sourcedir/include/media $dkmsdir/include-overrides/media
cp $sourcedir/include/uapi/linux/videodev2.h $dkmsdir/include-overrides/uapi/linux/videodev2.h
```