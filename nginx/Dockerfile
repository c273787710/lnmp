FROM centos:7

MAINTAINER cyj <273787710@qq.com>

ENV NGINX_VERSION 1.18.0

RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone

RUN set -x \
  && yum update -y \
  && yum install -y wget gcc gcc-c++ make automake autoconf libtool libxml2-devel libxslt-devel perl-devel perl-ExtUtils-Embed pcre-devel perl file tar bzip2-devel bzip2 openssl openssl-devel zlib zlib-devel gd-devel

ADD nginx-$NGINX_VERSION.tar.gz /home

WORKDIR /home

RUN wget http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz \
	&& tar zxf nginx-$NGINX_VERSION.tar.gz \
	&& groupadd nginx \
	&& useradd -r -g nginx nginx \
	&& cd nginx-$NGINX_VERSION \
	&& ./configure \
	--prefix=/usr/local/nginx \
	--user=nginx \
    --group=nginx \
	--with-http_ssl_module \
	--with-http_flv_module \
	--with-http_stub_status_module \
	--with-http_gzip_static_module \
	--with-http_realip_module \
	--with-http_image_filter_module \
	--with-pcre \
	--http-client-body-temp-path=/usr/local/nginx/tmp/client_body_temp \
	--http-fastcgi-temp-path=/usr/local/nginx/tmp/fastcgi_temp \
	--http-proxy-temp-path=/usr/local/nginx/tmp/proxy_temp \
	--http-uwsgi-temp-path=/usr/local/nginx/tmp/uwsgi_temp \
	--http-scgi-temp-path=/usr/local/nginx/tmp/scgi_temp \
	--with-http_v2_module \
	--with-threads \
    --with-stream \
    --with-stream_ssl_module \
    --with-http_addition_module \
    --with-http_sub_module \
    --with-http_dav_module \
    --with-http_mp4_module \
    --with-http_gunzip_module \
    --with-http_random_index_module \
    --with-http_secure_link_module \
    --with-http_auth_request_module \
    --with-mail \
    --with-mail_ssl_module \
    --with-file-aio \

	&& make && make install \
	&& mkdir /usr/local/nginx/tmp \
	&& yum clean all 

CMD ["/usr/local/nginx/sbin/nginx", "-g", "daemon off;"]

EXPOSE 80 443
