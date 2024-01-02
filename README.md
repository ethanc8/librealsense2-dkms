# librealsense2-dkms

This branch contains Intel's DKMS modules for kernels 5.15, 5.19, and 6.2 on Ubuntu 20.04 and Ubuntu 22.04.

The source was obtained through the following:

```bash
wget https://librealsense.intel.com/Debian/apt-repo/pool/jammy/main/librealsense2-dkms_1.3.24-0ubuntu1_all.deb
dpkg-deb -R ./librealsense2-dkms_1.3.24-0ubuntu1_all.deb librealsense2-dkms_1.3.24
```

You can build a Debian package from this source code using

```bash
git clone https://github.com/ethanc8/librealsense2-dkms --branch ubuntu-22.04
rm librealsense2-dkms/README.md
fakeroot dpkg-deb --build librealsense2-dkms librealsense2-dkms_1.3.24-6.2.0+ubuntu22.04.deb
```

## Using the files from the Ubuntu kernel

```bash
# Get the URL from https://packages.ubuntu.com/jammy-updates/all/linux-source-6.2.0/download
wget http://archive.ubuntu.com/ubuntu/pool/main/l/linux-hwe-6.2/linux-source-6.2.0_6.2.0-39.40~22.04.1_all.deb
dpkg-deb -R ./linux-source-6.2*.deb ubuntu-kernel-tarball
tar xjf ubuntu-kernel-tarball/usr/src/linux-source-6.2*.tar.bz2
# The kernel source is in linux-source-6.2.0/
dkmsdir=librealsense2-dkms/usr/src/librealsense2-dkms-1.3.24/6.2.0
sourcedir=linux-source-6.2.0
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