```
yum install zbar zbar-devel
apt-get zbar-tools libzbar-dev libv4l-dev libqtcore4 libqtgui4
```
https://github.com/jorissteyn/php-zbarcode/archive/gophp7.zip
```
cd /usr/include/linux
sudo ln -s ../libv4l1-videodev.h videodev.h

sudo ln -s /usr/local/webserver/imagemagick/include/ImageMagick-7/ /usr/local/include/ImageMagick
sudo ln -s /usr/local/webserver/imagemagick/include/ImageMagick-7/MagickWand /usr/local/include/wand

export CFLAGS=""
./congfig --prefix=/usr/local/webserver/zbar --without-gtk --without-python --without-qt
```
```
./configure --with-zbarcode=/usr/local/webserver/zbar --with-zbarcode-imagemagick-dir=/usr/local/webserver/imagemagick
```