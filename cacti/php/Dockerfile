FROM centos:7
MAINTAINER linuxwt <tengwanginit@gmail.com>

# 更换163源
RUN yum -y install wget
RUN cd /etc/yum.repos.d \
&&  mv CentOS-Base.repo CentOS-Base.repo.bak \
&& wget  http://mirrors.163.com/.help/CentOS7-Base-163.repo \ 
&& mv CentOS7-Base-163.repo CentOS-Base.repo
RUN yum makecache && yum -y update

# 安装依赖
RUN yum -y install gcc gcc-c++
RUN yum -y install make

ADD libmcrypt-2.5.7.tar.gz /tmp 
WORKDIR /tmp/libmcrypt-2.5.7
RUN ./configure && make && make install

ADD php-5.6.38.tar.gz /tmp
RUN yum -y install perl-devel ibxml2 libxml2-devel bzip2 bzip2-devel libjpeg-turbo libjpeg-turbo-devel libpng libpng-devel freetype freetype-devel zlib zlib-devel libcurl libcurl-devel  openssl openssl-devel openldap openldap-devel 


WORKDIR /tmp/php-5.6.38
RUN ./configure --prefix=/usr/local/php --with-pdo-mysql=mysqlnd --with-mysqli=mysqlnd --with-mysql=mysqlnd --with-openssl --enable-mbstring --with-freetype-dir --with-jpeg-dir --with-png-dir --with-mcrypt --with-zlib --with-libxml-dir=/usr --enable-xml  --enable-sockets --enable-fpm --with-config-file-path=/usr/local/php/etc --with-bz2 --with-gd  --enable-gd-native-ttf  --enable-sockets --enable-mbstring --enable-bcmath --without-pear  --disable-phar && make && make install

RUN wget http://pear.php.net/go-pear.phar 
RUN /usr/local/php/bin/php go-pear.phar


#RUN cd /tmp && wget http://ftp.gnu.org/gnu/m4/m4-1.4.9.tar.gz && tar zvxf m4-1.4.9.tar.gz && cd m4-1.4.9/ && ./configure && make && make install 
#RUN cd /tmp && wget http://ftp.gnu.org/gnu/autoconf/autoconf-2.62.tar.gz && tar -zvxf autoconf-2.62.tar.gz && cd autoconf-2.62/ && ./configure && make && make install

RUN  yum -y install m4
RUN  yum -y install autoconf

RUN yum -y install net-snmp net-snmp-devel
RUN cp -frp /usr/lib64/libldap* /usr/lib && yum -y install gmp-devel && cd /tmp/php-5.6.38/ext/ldap && /usr/local/php/bin/phpize && ./configure --with-php-config=/usr/local/php/bin/php-config && make && make install

RUN cd /tmp/php-5.6.38/ext/snmp && /usr/local/php/bin/phpize && ./configure --with-php-config=/usr/local/php/bin/php-config && make && make install
RUN cd /tmp/php-5.6.38/ext/gmp && /usr/local/php/bin/phpize && ./configure --with-php-config=/usr/local/php/bin/php-config && make && make install


RUN cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf 

RUN useradd -M -s /sbin/nologin php
RUN sed -i -e 's@;pid = run/php-fpm.pid@pid = run/php-fpm.pid@g' -e 's@nobody@php@g' -e 's@listen = 127.0.0.1:9000@listen = 0.0.0.0:9000@g' /usr/local/php/etc/php-fpm.conf
RUN sed -i 's@;daemonize = yes@daemonize = no@g' /usr/local/php/etc/php-fpm.conf
EXPOSE 9000

CMD ["/usr/local/php/sbin/php-fpm"]
