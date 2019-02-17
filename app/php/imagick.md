```
yum install ImageMagick ImageMagick-devel
```
```
tar xf ImageMagick-7.0.6-5.tar.gz
cd ImageMagick-7.0.6-5
./configure --prefix=/usr/local/webserver/imagemagick
make -j 4 && make install 
export PKG_CONFIG_PATH=/usr/local/webserver/imagemagick/lib/pkgconfig/  #设置环境变量
```
```
wget http://pecl.php.net/get/imagick-3.4.3.tgz
apt-get install autoconf && 
export PKG_CONFIG_PATH=/usr/local/webserver/imagemagick/lib/pkgconfig/
&& cd /usr/local/src \
&& tar -zxvf imagick-3.4.3.tgz \
&& cd imagick-3.4.3 \
&& /usr/local/webserver/php7/bin/phpize \
&& ./configure  \
--with-php-config=/usr/local/webserver/php7/bin/php-config \
--with-imagick=/usr/local/webserver/imagemagick \
&& make -j 8 \
&& make install 
```
