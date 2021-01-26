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

##### 7. 挂载cephfs到本地
```sh
mount -t ceph xxx.xxx.xxx.xxx:/ /test -o name=admin,secret=xxxxxxxxxx==
```

##### 8. MySQL编译
```sh
wget https://github.com/mysql/mysql-server/archive/mysql-5.7.32.tar.gz
tar xzvf mysql-5.7.32.tar.gz
cd mysql-5.7.32
cmake . -DCMAKE_INSTALL_PREFIX=/opt/mysql/mysql -DMYSQL_DATADIR=/opt/mysql/data -DWITH_BOOST=/usr/local/boost -DSYSCONFDIR=/opt/mysql -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_FEDERATED_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_MYISAM_STORAGE_ENGINE=1 -DENABLED_LOCAL_INFILE=1 -DENABLE_DTRACE=0 -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EMBEDDED_SERVER=1
make -j 2
make install
```

##### 9. ceph编译
```sh
#公共
git clone --branch v15.2.8 https://github.com/ceph/ceph.git
cd ceph
./install-deps.sh
#指定cypress包
export CYPRESS_INSTALL_BINARY=/opt/cypress.zip

#编译
ARGS="-DCMAKE_C_COMPILER=gcc" ./do_cmake.sh -DCMAKE_BUILD_TYPE=RelWithDebInfo
make

#安装
make install

#生成rpm包
./make-srpm.sh
mkdir ~/rpmbuild/{BUILD,SOURCES,SPECS,RPMS,BUILDROOT}
cp ceph/ceph-15.2.8-0.*.src.rpm ~/rpmbuild/SOURCES
cd ~/rpmbuild/SOURCES
rpm2cpio ceph-15.2.8-0.*.src.rpm | cpio -idmv
mv ceph.spec ../SPECS
rpmbuild -ba ~/rpmbuild/SPECS/ceph.spec
```

##### 10. 获取consul配置
```sh
curl -X GET http://{ip}:{port}/v1/kv/config/global_config?token={token} -s | jq .[0] | jq -r .Value | base64 -d
```

##### 11. 扩容VirtualBox存储
```sh
C:\Program Files\Oracle\VirtualBox>VBoxManage modifyhd "E:\VirtualBox VMs\Centos7_B\Centos7_B.vdi" --resize 40960
#刷新磁盘
partprobe
xfs_growfs  /dev/centos/root
```
