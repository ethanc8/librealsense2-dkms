# librealsense2-dkms

This branch contains Intel's DKMS modules for kernels 5.15, 5.19, and 6.2 on Ubuntu 20.04 and Ubuntu 22.04.

The source was obtained through the following:

```bash
wget https://librealsense.intel.com/Debian/apt-repo/pool/jammy/main/librealsense2-dkms_1.3.24-0ubuntu1_all.deb
dpkg-deb -R ./librealsense2-dkms_1.3.24-0ubuntu1_all.deb librealsense2-dkms_1.3.24
```

You can build a Debian package from this source code using

```bash
git clone https://github.com/ethanc8/librealsense2-dkms --branch debian-12
rm librealsense2-dkms/README.md
fakeroot dpkg-deb --build librealsense2-dkms librealsense2-dkms_1.3.24-1+6.1.69+debian12.deb
```

## Using the files from the Debian kernel

```bash
# Get the URL from https://packages.debian.org/bookworm/all/linux-source-6.1/download
wget http://security.debian.org/debian-security/pool/updates/main/l/linux/linux-source-6.1_6.1.69-1_all.deb
dpkg-deb -R ./linux-source-6.1*.deb debian-kernel-tarball
tar xaf debian-kernel-tarball/usr/src/linux-source-6.1.tar.xz
# The kernel source is in linux-source-6.1/
dkmsdir=librealsense2-dkms/usr/src/librealsense2-dkms-1.3.24/6.1.0
sourcedir=linux-source-6.1
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