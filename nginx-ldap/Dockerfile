FROM centos:6

RUN yum -y install gcc gcc-c++ make zlib-devel pcre-devel openssl-devel wget tar git openldap openldap-devel php5-fpm php5-pgsql

RUN cd ~/ && \
    wget http://nginx.org/download/nginx-1.9.0.tar.gz && \
    tar -xvzf nginx-1.9.0.tar.gz && \
    git clone https://github.com/kvspb/nginx-auth-ldap.git ~/nginx-auth-ldap && \
    cd ~/nginx-1.9.0 && \
    ./configure --add-module=../nginx-auth-ldap \
                --with-http_gzip_static_module \
                --with-http_stub_status_module \
                --with-http_ssl_module \
                --with-pcre \
                --with-file-aio \
                --with-http_realip_module \
                --error-log-path=/var/log/nginx/error.log \
                --http-log-path=/var/log/nginx/access.log \
                --prefix=/etc/nginx && \
    make && \
    make install


# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log


VOLUME ["/var/cache/nginx"]

EXPOSE 80 443

CMD ["/etc/nginx/sbin/nginx", "-g", "daemon off;"]