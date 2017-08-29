FROM local/sshdcentos:7
MAINTAINER by chenrixiang(ri.xiang.c@gmail.com)


#Tasr start

RUN mkdir -p /usr/local/data 

#git Tars
#RUN cd /usr/local/data & git clone https://github.com/Tencent/Tars.git
ADD Tars /usr/local/data/Tars

#RUN cd /usr/local/data & git push



#Tars end


#resin
ADD resin-pro-4.0.53 /usr/local/resin-pro-4.0.53
RUN ln -s /usr/local/resin-pro-4.0.53 /usr/local/resin



RUN chmod -R 755 /usr/local/maven


#SETTING JDK PATH
ENV JAVA_HOME /usr/local/java/jdk1.8

ENV PATH /usr/local/java/jdk1.8/bin:/usr/local/maven/apache-maven-3.5.0/bin:$PATH

RUN java -version

WORKDIR /usr/local/data/Tars/java

RUN mvn install 
RUN mvn clean install -f core/client.pom.xml 
RUN mvn clean install -f core/server.pom.xml



#RUN yum install -y flex

#WORKDIR /usr/local/data/Tars/cpp/build
#RUN chmod u+x build.sh
#RUN ./build.sh all

WORKDIR /usr/local

RUN mkdir tars

#WORKDIR /usr/local/data/Tars/cpp/build
#RUN ./build.sh install

WORKDIR /usr/local/data/Tars/web

RUN mvn clean package

RUN cp /usr/local/data/Tars/web/target/tars.war /usr/local/resin/webapps 



RUN mkdir -p /usr/local/app/patchs/tars.upload/
RUN mkdir -p /data/log/tars
RUN mkdir -p /usr/local/tars/cpp
RUN mkdir -p /usr/local/app/tars

RUN chmod -R 755 /usr/local/app/tars


WORKDIR /usr/local/data/Tars/cpp/build
RUN chmod u+x build.sh
RUN ./build.sh all
RUN ./build.sh install

RUN make framework-tar
RUN make tarsstat-tar
RUN make tarsnotify-tar
RUN make tarsproperty-tar
RUN make tarslog-tar
RUN make tarsquerystat-tar
RUN make tarsqueryproperty-tar

RUN cp ./framework.tgz /usr/local/app/tars/

WORKDIR /usr/local/app/tars

RUN tar xzfv framework.tgz





RUN sed -i "s/192.168.2.131/172.18.0.2/g" `grep 192.168.2.131 -rl ./*`
RUN sed -i "s/db.tars.com/172.18.0.2/g" `grep db.tars.com -rl ./*`
RUN sed -i "s/registry.tars.com/172.18.0.2/g" `grep registry.tars.com -rl ./*`
RUN sed -i "s/web.tars.com/172.18.0.2/g" `grep web.tars.com -rl ./*`

RUN chmod u+x tars_install.sh




#将ssh服务启动脚本复制到/usr/local/sbin目录中，并改变权限为755
ADD run.sh /usr/local/sbin/run.sh
RUN chmod -R 755 /usr/local/sbin/run.sh
#设置生成容器时需要执行的脚本
CMD ["/usr/local/sbin/run.sh"]

CMD ["/usr/sbin/init"]

#开放22、80、443端口
EXPOSE 22 3306 80 443 8080 6600 6800
