# Tars     https://github.com/Tencent/Tars/



    服务器没有足够的资源跑tars ，而tars在ox 下有些坑填不上，刚好又进了docker坑，新手上路，学了一点皮毛干脆（zhuang bi）就在上面搭了个环境，在linux centos 7 下试过搭建了几次tars环境了，该注意的点也被坑过，庆幸能在 docker 给跑了起来了
    不过好像docker hub 上已经有tars 的docker 版本了，自当学习的，搞完还得去围观学习别人是怎么整的，仰慕一下。


 ![image](https://github.com/nigly/docker_tars/blob/master/images/1.png)


动手前 
```Bash

cd ./sshcentos7 
git clone https://github.com/alibaba/AliSQL.git

maven jdk 等下载对应版本或者其他版本，但需要修改Dockerfile 


cd ./tars 
git clone https://github.com/Tencent/Tars.git

resin 等下载对应版本或者其他版本，但需要修改Dockerfile 

```
    
    

#Docker 

安装docker </br>
启动 docker

1、拉取 Centos
```Bash
docker pull centos:7
``` 
2、Yum install 各种库依赖等
```
cd ./centos7
docker build --rm -t local/centos:7 .
```
3、ssh mysql jdk git maven 等环境配置
```Bash
cd ./sshcentos7
docker build --rm -t local/sshdcentos:7 .
-- test ssh centos 
docker run -d -p 10022:22 -p 33066:3306 local/sshdcentos:7 /usr/local/sbin/run.sh
ssh root@ip -p 10022
password:147258
```
4、Tars编译配置
```Bash
cd ./tars
docker build --rm -t local/tars:1.0 .
```
5、最后运行容器
```Bash
-- 固定ip
docker network create --subnet=172.18.0.0/16 shadownet

-- 固定容器ip 172.18.0.2 
docker run --privileged -d -p 10022:22 -p 33066:3306 -p 18080:8080 --net shadownet --ip 172.18.0.2 local/tars:1.0 /usr/local/sbin/run.sh /usr/sbin/init
    
    
-- ssh login
ssh root@192.168.5.103 -p 10022
        
```

192.168.5.103 为当前mac 的ip 根据 自己mac 的ip来变更修改吧

至此 可以ssh 登录到centos系统

还得继续，一些mysql的权限配置，启动没在Dockerfile 里面配置（因为我不会 <_>  ）

```Bash

# /etc/init.d/mysqld start
Starting MySQL. SUCCESS! 
-- 启动完成啦

```



    
    
    


待整理

