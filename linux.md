##### 1. 进程信息显示不全
```sh
ps -ef  #查询进程号 
cat /proc/{进程号}/cmdline
```

##### 2. 查询已删除文件但是占用磁盘空间进程
```sh
lsof | grep deleted (比较耗时)
find /proc/*/fd -ls | grep '(deleted)'
```

##### 3. 分割文件
```sh
split -b {size(K/M/G)} {input file} {output file}
cat {output file}* > {input file}

PS: window
COPY /b {output file}* {input file}
```

##### 4. tcpdump抓包
```sh
tcpdump -i bond0  port 80 -ennA
```

##### 5. centos7安装MySQL
```sh
wget https://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
rpm -ivh mysql57-community-release-el7-8.noarch.rpm
yum install mysql-server
```

##### 6. 查找软件包所有版本
```sh
yum list –showduplicates {name}
```
