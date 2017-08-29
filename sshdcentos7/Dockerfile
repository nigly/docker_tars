FROM local/centos:7
MAINTAINER by chenrixiang(ri.xiang.c@gmail.com)
#RUN yum install -y openssh-server openssh-clients wget
RUN rm -f /etc/ssh/*_key 
RUN rm -f /etc/ssh/*_key.pub 
RUN ssh-keygen -q -t rsa -b 2048 -f /etc/ssh/ssh_host_rsa_key -N ''
RUN ssh-keygen -q -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ''
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_ed25519_key  -N ''
RUN sed -i "s/#UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config
RUN sed -i "s/UsePAM.*/UsePAM no/g" /etc/ssh/sshd_config
#将ssh服务启动脚本复制到/usr/local/sbin目录中，并改变权限为755
ADD run.sh /usr/local/sbin/run.sh
RUN chmod 755 /usr/local/sbin/run.sh
#变更root密码为147258
RUN echo "root:147258"|chpasswd


#安装jdk

ADD jdk1.8.0_144 /usr/local/java/jdk1.8.0_144
RUN ln -s /usr/local/java/jdk1.8.0_144 /usr/local/java/jdk1.8 


#SETTING JDK PATH
RUN echo "export JAVA_HOME=/usr/local/java/jdk1.8" >>/etc/profile
RUN echo -e "export CLASSPATH=.:/usr/local/java/jdk1.8/lib" >>/etc/profile


#MAVEN
ADD apache-maven-3.5.0 /usr/local/maven/apache-maven-3.5.0

#SETTING MAVEN PATH
RUN echo "export M2_HOME=/usr/local/maven/apache-maven-3.5.0" >>/etc/profile
#RUN echo "export MAVEN_OPTS=-Xms256m-Xmx512m" >>/etc/profile


RUN echo "export PATH=/usr/local/java/jdk1.8/bin:/usr/local/maven/apache-maven-3.5.0/bin:$PATH" >>/etc/profile
RUN echo "export JAVA_HOME CLASSPATH M2_HOME PATH " >>/etc/profile
RUN source /etc/profile

#RUN yum -y install git


#RUN yum install -y glibc-devel gcc gcc-c++ automake autoconf libtool make


#cmake-3.9.1
#ADD cmake-3.9.1 /usr/local/cmake-3.9.1
#ADD cmake-2.8.12 /usr/local/cmake-2.8.12
#ADD cmake-2.8.10.2 /usr/local/cmake-2.8.10.2


#RUN ln -s /usr/local/cmake-3.9.1 /usr/local/cmake
#RUN ln -s /usr/local/cmake-2.8.12 /usr/local/cmake
#RUN ln -s /usr/local/cmake-2.8.10.2 /usr/local/cmake



#RUN cd /usr/local/cmake & ./bootstrap
#RUN cd /usr/local/cmake & make
#RUN cd /usr/local/cmake & make install


#bison-2.7
#ADD bison-2.7 /usr/local/bison-2.7


#AliSQL
ADD AliSQL /usr/local/data/AliSQL


RUN yum install centos-release-scl -y
RUN yum install devtoolset-4-gcc-c++ devtoolset-4-gcc -y
RUN yum install cmake bison ncurses ncurses-devel ncurses-libs openssl-devel -y





#WORKDIR /usr/local/cmake
#RUN ./bootstrap
#RUN make
#RUN make install

WORKDIR /usr/local

#RUN yum install bison-2.7

RUN yum install -y perl

#RUN yum install -y ncurses ncurses-devel ncurses-libs

RUN yum install initscripts -y



WORKDIR /usr/local

RUN groupadd mysql 
RUN useradd -r -g mysql mysql

RUN mkdir -p /usr/local/mysql/data
RUN mkdir -p /usr/local/mysql

RUN mv /usr/local/data/AliSQL/* /usr/local/mysql


WORKDIR /usr/local/mysql
RUN chown -R mysql:mysql .
RUN chmod -R 755 /usr/local/mysql


RUN scl enable devtoolset-4 bash

RUN ls -l -a 

RUN cmake .                           \
 -DCMAKE_BUILD_TYPE="Release"         \
 -DCMAKE_INSTALL_PREFIX="/usr/local/mysql" \
 -DWITH_EMBEDDED_SERVER=0             \
 -DWITH_EXTRA_CHARSETS=all            \
 -DWITH_MYISAM_STORAGE_ENGINE=1       \
 -DWITH_INNOBASE_STORAGE_ENGINE=1     \
 -DWITH_PARTITION_STORAGE_ENGINE=1    \
 -DWITH_CSV_STORAGE_ENGINE=1          \
 -DWITH_ARCHIVE_STORAGE_ENGINE=1      \
 -DWITH_BLACKHOLE_STORAGE_ENGINE=1    \
 -DWITH_FEDERATED_STORAGE_ENGINE=1    \
 -DWITH_PERFSCHEMA_STORAGE_ENGINE=1   \
 -DWITH_TOKUDB_STORAGE_ENGINE=1

RUN make && make install



WORKDIR /usr/local/mysql
RUN chown -R mysql:mysql .

WORKDIR /usr/local/mysql/data
RUN chown -R mysql:mysql .


WORKDIR /usr/local/mysql
RUN scripts/mysql_install_db --user=mysql --datadir=/usr/local/mysql/data



#RUN echo "user=mysql" >>/usr/local/mysql/my.cnf
#RUN echo "basedir =/usr/local/mysql" >>/usr/local/mysql/my.cnf
#RUN echo "datadir =/usr/local/mysql/data" >>/usr/local/mysql/my.cnf
#RUN echo "bind-address=192.168.5.103" >>/usr/local/mysql/my.cnf

ADD my.cnf /usr/local/mysql/my.cnf

RUN cat /usr/local/mysql/my.cnf


RUN chmod -R 755 /usr/local/mysql/data
#RUN /usr/local/mysql/support-files/mysql.server start


RUN cp /usr/local/mysql/support-files/mysql.server  /etc/init.d/mysqld
RUN ln -s /usr/local/mysql/my.cnf /etc/my.cnf


#RUN chkconfig --level 35 mysqld on

#RUN /etc/init.d/mysqld start



#RUN ps -ef | grep mysql






#开放22、80、443端口
EXPOSE 22 3306 80 443 8080
#运行脚本，启动sshd服务
CMD ["/usr/local/sbin/run.sh"]

CMD ["/usr/sbin/init"]

CMD ["source /etc/profile"]


