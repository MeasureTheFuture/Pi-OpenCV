# Pi-OpenCV

How to build OpenCV 3.0 binaries for the Raspberry Pi.

## Compilation of OpenCV 3.0 on the Raspberry Pi

```
$ sudo apt-get update && sudo apt-get upgrade
$ sudo apt-get install build-essential cmake pkg-config
$ sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
$ sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
$ sudo apt-get install libxvidcore-dev libx264-dev
$ sudo apt-get install libgtk2.0-dev libgtk-3-dev
$ sudo apt-get install libatlas-base-dev gfortran
$ cd ~
$ wget -O opencv.zip https://github.com/opencv/opencv/archive/3.4.1.zip
$ unzip opencv.zip
$ wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/3.4.1.zip
$ unzip opencv_contrib.zip
$ cd ~/opencv-3.4.1/
$ mkdir build
$ git clone https://github.com/MeasureTheFuture/Pi-OpenCV.git
$ cd build
$ cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_PYTHON_EXAMPLES=OFF \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-3.4.1/modules \
    -D BUILD_EXAMPLES=OFF ..
```

Edit swap space:
```
# set size to absolute value, leaving empty (default) then uses computed value
#   you most likely don't want this, unless you have an special disk situation
# CONF_SWAPSIZE=100
CONF_SWAPSIZE=1024
```
Restart swapfile
```
$ sudo /etc/init.d/dphys-swapfile stop
$ sudo /etc/init.d/dphys-swapfile start
```

Build OpenCV (This will take a couple of hours)
```
$ make -j4
```

## Create debian package
```
$ sudo apt-get install dh-make
DEB_BUILD_OPTIONS=nocheck dpkg-buildpackage -uc -tc -rfakeroot -nc -j4
```

## References
* [pyimagesearch](https://www.pyimagesearch.com/2017/09/04/raspbian-stretch-install-opencv-3-python-on-your-raspberry-pi/)