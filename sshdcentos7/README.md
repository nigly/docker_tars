3、ssh mysql jdk git maven 等环境配置
```Bash
cd ./sshcentos7
docker build --rm -t local/sshdcentos:7 .
-- test ssh centos 
docker run -d -p 10022:22 -p 33066:3306 local/sshdcentos:7 /usr/local/sbin/run.sh
ssh root@ip -p 10022
password:147258
```
