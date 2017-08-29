# Tars     https://github.com/Tencent/Tars/



服务器没有资源一直跑tars ，tars 在mac 环境也是伤脑筋，刚好又学了docker 一点皮毛 干脆就在上面搭了个环境，在linux centos 7 下跑了几次tars 了，所以用了3天 把 docker 给跑了起来 


 ![image](https://github.com/nigly/docker_tars/blob/master/images/1.png)

    

shell

       
    cd ./local/centos7
    docker build --rm -t local/centos:7 .


2、ssh 登录

      cd ./local/sshcentos7
    docker build --rm -t local/sshdcentos:7 .

3、
docker run -d -p 10022:22 -p 33066:3306 local/sshdcentos:7 /usr/local/sbin/run.sh


docker build --rm -t local/system-centos:7 .


docker run --privileged -d -p 10022:22 -p 33066:3306 local/system-centos:7 /usr/local/sbin/run.sh /usr/sbin/init




docker build --rm -t local/system-centos:alisql .



docker build --rm -t local/tars:1.0 .


docker build --rm -t local/tars:1.1.0 .


docker run --privileged -d -p 10022:22 -p 33066:3306 -p 18080:8080 -p 16600:6600 -p 16800:6800 --net shadownet --ip 172.18.0.2 local/tars:1.0 /usr/local/sbin/run.sh /usr/sbin/init


ssh root@192.168.5.103 -p 10022



docker network create --subnet=172.18.0.0/16 shadownet




 /usr/local/mysql/support-files/mysql.server start;
./bin/mysqladmin -u root password ‘123456'
mysqladmin -u root password "123456” 

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;


flush privileges;
    
    
    


待续

