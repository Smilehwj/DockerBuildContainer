FROM centos:centos7

ENV apr_version=1.7.0 apr_util_version=1.6.1 httpd_version=2.4.54 

RUN yum update -y
RUN yum install -y make gcc gcc-c++ openssl-devel pcre-devel expat-devel libtool libxml2-devel wget sudo git

RUN mkdir /home/build
WORKDIR /home/build

# install apr
RUN cd /home/build
RUN wget https://mirrors.aliyun.com/apache/apr/apr-${apr_version}.tar.gz
RUN tar -xvzf apr-${apr_version}.tar.gz
RUN cd apr-${apr_version} \
    && sudo ./configure --prefix=/usr/local/src/apr \
    && sudo make && make install
RUN cd /etc/ld.so.conf.d/
RUN echo /usr/local/src/apr/lib/ >/etc/ld.so.conf.d/apr.conf
RUN ldconfig 
RUN ln -s /usr/local/src/apr/include/ /usr/include/apr
# RUN ll /usr/include/apr

# install apr-util
RUN cd /home/build
RUN wget https://mirrors.aliyun.com/apache/apr/apr-util-${apr_util_version}.tar.gz
RUN mkdir /home/build/apr-util-${apr_util_version}
RUN tar -xvzf apr-util-${apr_util_version}.tar.gz
RUN cd apr-util-${apr_util_version} \
    && sudo ./configure --prefix=/usr/local/src/apr-util --with-apr=/usr/local/src/apr \
    && sudo make && make install
RUN cd /etc/ld.so.conf.d/
RUN echo /usr/local/src/apr-util/ >/etc/ld.so.conf.d/apr-util.conf 
RUN ldconfig 
RUN ln -s /usr/local/src/apr-util/include/ /usr/include/apr-util
# RUN ll /usr/include/apr-util

# install apache2.4.54
RUN cd /home/build
RUN wget https://mirrors.aliyun.com/apache/httpd/httpd-${httpd_version}.tar.gz
RUN mkdir httpd-${httpd_version}
RUN tar -xvzf httpd-${httpd_version}.tar.gz
RUN cd httpd-${httpd_version} \
    && sudo ./configure --prefix=/usr/local/src/httpd --with-apr=/usr/local/src/apr --with-apr-util=/usr/local/src/apr-util \
    && sudo make && make install

# delete workdir
RUN rm -fr /home/build