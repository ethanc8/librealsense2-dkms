# librealsense2-dkms

This branch contains Intel's DKMS modules for kernels 5.15, 5.19, and 6.2 on Ubuntu 20.04 and Ubuntu 22.04.

The source was obtained through the following:

```bash
wget https://librealsense.intel.com/Debian/apt-repo/pool/jammy/main/librealsense2-dkms_1.3.24-0ubuntu1_all.deb
dpkg-deb -R ./librealsense2-dkms_1.3.24-0ubuntu1_all.deb librealsense2-dkms_1.3.24
```

You can build a Debian package from this source code using

```bash
git clone https://github.com/ethanc8/librealsense2-dkms
dpkg-deb --build librealsense2-dkms librealsense2-dkms_1.3.24-0ubuntu1_all.deb
```