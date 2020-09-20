FROM centos:7
MAINTAINER cyj <273787710@qq.com>

ENV PHP_VERSION 7.3.8
RUN yum install -y curl \
	&& curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo \
	&& yum clean all \
	&& yum remove oniguruma-devel -y \
	&& yum remove oniguruma -y \
	&& rpm -ivh https://www.chenleilei.net/soft/docker/oniguruma-6.7.0-1.el7.x86_64.rpm \
	&& rpm -ivh https://www.chenleilei.net/soft/docker/oniguruma-devel-6.7.0-1.el7.x86_64.rpm \
	&& yum install -y cmake pcre pcre-devel openssl openssl-devel gd-devel zlib-devel gcc gcc-c++ net-tools iproute telnet wget curl wget vim libxml2 libxml2-devel openssl openssl-devel libjpeg-turbo libjpeg-turbo-devel libpng-devel libpng freetype-devel freetype icu libicu-devel libicu libmcrypt libmcrypt-devel libxslt libxslt-devel  php-mysql libsqlite3x-devel \ 
	&& rm -rf /var/cache/yum/*

WORKDIR /

RUN useradd www -u 1200 -M -s /sbin/nologin \
	&& mkdir -p /usr/local/nginx/html \
	&& chown -R www.www /usr/local/nginx/html \
	&& chmod -R 755 /usr/local/nginx/html

ADD php-$PHP_VERSION.tar.gz /home

WORKDIR /home/php-$PHP_VERSION
RUN ./configure --prefix=/usr/local/php \
	--with-config-file-path=/usr/local/php/etc \
	--with-config-file-scan-dir=/usr/local/php/conf.d \
	--enable-fpm --with-fpm-user=www \
	--with-fpm-group=www \
	--with-mysqli=mysqlnd \
	--with-pdo-mysql=mysqlnd \
	--with-iconv-dir \
	--with-zlib \
	--enable-xml \
	--disable-rpath \
	--enable-bcmath \
	--enable-shmop \
	--enable-sysvsem \
	--enable-inline-optimization \
	--enable-mbregex \
	--enable-mbstring \
	--enable-ftp \
	--with-openssl \
	--with-mhash \
	--enable-pcntl \
	--enable-sockets \
	--with-xmlrpc \
	--enable-soap \
	--with-gettext \
	--disable-fileinfo \
	--enable-opcache \
	--enable-intl \
	--with-xsl \
	&& make -j 4 && make install \
	&& cp ./sapi/fpm/php-fpm.conf /usr/local/php/etc/php-fpm.conf \
	&& cp ./php.ini-production /usr/local/php/etc/php.ini \
	&& cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf \
	&& sed  -i  "s#127.0.0.1:9000#0.0.0.0:9000#g" /usr/local/php/etc/php-fpm.d/www.conf \
	&& sed -in '99idaemonize = yes' /usr/local/php/etc/php-fpm.conf \
	&& cp ./sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm  \
	&& chmod a+x /etc/init.d/php-fpm \
	&& ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

CMD /usr/local/php/sbin/php-fpm -F

EXPOSE 9000