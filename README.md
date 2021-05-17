# Dockerfile
FROM centos:7
MAINTAINER "NaMe ZengFangQuan <BLOG www.gylinux.cn>"
RUN yum install -y gcc gcc-c++ make openssl-devel pcre-devel gd-devel \
&& yum install -y iproute net-tools telnet wget curl \
&& yum clean all \
&& rm -rf /var/cache/yum/*
ADD nginx-1.20.0.tar.gz /usr/local/
RUN cd /usr/local/nginx-1.20.0 \
&& ./configure --prefix=/usr/local/nginx \
--with-http_ssl_module \
--with-http_stub_status_module \
&& make -j 4 \
&& make install \
&& mkdir /usr/local/nginx/conf/vhost \
&& cd / \
&& rm -rf nginx* \
&& ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
ENV PATH $PATH:/usr/local/nginx/sbin
COPY nginx.conf /usr/local/nginx/conf/nginx.conf
WORKDIR /usr/local/nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
