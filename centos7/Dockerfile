#生成的新镜像以centos镜像为基础
FROM centos:7
MAINTAINER chenrixiang<ri.xiang.c@gmail.com>
#更新系统
RUN yum -y update && yum clean all
RUN yum install -y openssh-server openssh-clients wget git glibc-devel gcc gcc-c++ ncurses-devel automake autoconf libtool make

RUN yum install -y centos-release-scl devtoolset-4-gcc-c++ devtoolset-4-gcc ncurses-devel openssl-devel bison flex

CMD ["/usr/sbin/init"]
