# Tars     https://github.com/Tencent/Tars/



服务器没有资源一直跑tars ，tars 在mac 环境也是伤脑筋，刚好又学了docker 一点皮毛 干脆就在上面搭了个环境，在linux centos 7 下跑了几次tars 了，所以用了3天 把 docker 给跑了起来 


 ![image](https://github.com/nigly/docker_tars/blob/master/images/1.png)

    
安装docker
启动 docker

1、
···
docker pull centos:7
···
    
2、

    cd ./centos7
    docker build --rm -t local/centos:7 .
3、

    cd ./sshcentos7
    
    
    docker build --rm -t local/sshdcentos:7 .

    test ssh centos 
    
    
    docker run -d -p 10022:22 -p 33066:3306 local/sshdcentos:7 /usr/local/sbin/run.sh
    
    
    ssh root@ip -p 10022
    
    
    password:147258

4、

    cd ./tars
    docker build --rm -t local/tars:1.0 .

5、

    --固定ip
    
    docker network create --subnet=172.18.0.0/16 shadownet
    
    -- 固定容器ip 172.18.0.2
    
    docker run --privileged -d -p 10022:22 -p 33066:3306 -p 18080:8080 --net shadownet --ip 172.18.0.2 local/tars:1.0 /usr/local/sbin/run.sh /usr/sbin/init
    
    
    ssh login
    
    
    ssh root@192.168.5.103 -p 10022
        
    




    
    
    


待整理

