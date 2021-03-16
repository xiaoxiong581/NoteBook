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
tcpdump -i enp0s8 -s 0 -l -w - dst port 3402 | strings
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
```txt
要求：
1、cmake 要高于3.10.2 版本
2、gcc版本要 >= 8(编译过程会自动安装，可暂时不提前升级)
3、磁盘空间保证至少50G左右
4、内存至少8G左右，cpu 2核以上
```
```sh
#公共
git clone --branch v15.2.8 https://github.com/ceph/ceph.git
cd ceph
./install-deps.sh
#指定cypress包
wget https://cdn.cypress.io/desktop/4.4.0/linux-x64/cypress.zip
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

##### 10. 下载ceph离线安装包
```sh
#安装工具包
yum -y install epel-release
yum -y install yum-utils
yum -y install createrepo

#在可连外网的主机上配置ceph的repo
[ceph]
name=ceph
baseurl=http://mirrors.aliyun.com/ceph/rpm-octopus/el7/x86_64/
gpgcheck=0
[ceph-noarch]
name=cephnoarch
baseurl=http://mirrors.aliyun.com/ceph/rpm-octopus/el7/noarch/
gpgcheck=0
[ceph-source]
name=cephsource
baseurl=http://mirrors.aliyun.com/ceph/rpm-octopus/el7/x86_64/
gpgcheck=0
[ceph-radosgw]
name=cephradosgw
baseurl=http://mirrors.aliyun.com/ceph/rpm-octopus/el7/x86_64/
gpgcheck=0

#下载ceph离线包
mkdir ceph_v15.2.8
repotrack ceph ceph-mon ceph-mgr ceph-radosgw ceph-osd ceph-deploy -p ceph_v15.2.8/

#生成用于离线安装的仓库元数据
createrepo -v ceph_v15.2.8

#离线主机配置ceph安装目录
[Local-ceph]
name=ceph 13.2.10
baseurl=file:///home/xxx/ceph_v15.2.8
enabled=1
gpgcheck=0
```

##### 11. 获取consul配置
```sh
curl -X GET http://{ip}:{port}/v1/kv/config/global_config?token={token} -s | jq .[0] | jq -r .Value | base64 -d
# 删除服务
curl -L -X PUT -H "X-Consul-Token: $CONSUL_HTTP_TOKEN" --data '{"Node": "bypass-route", "ServiceID": "vault-sidecar-proxy"}'  http://localhost:8500/v1/catalog/deregister
# 查询服务
curl -L -X GET -H "X-Consul-Token: $CONSUL_HTTP_TOKEN" http://localhost:8500/v1/catalog/service/{serviceName}
```

##### 12. 扩容VirtualBox存储
```sh
C:\Program Files\Oracle\VirtualBox>VBoxManage modifyhd "E:\VirtualBox VMs\Centos7_B\Centos7_B.vdi" --resize 40960
#刷新磁盘
partprobe
xfs_growfs  /dev/centos/root
```

##### 13. 文件句柄数
```sh
# 系统最大句柄数
# echo 123456 > /proc/sys/fs/file-max 可临时修改
# fs.file-max = 123456 添加在/etc/sysctl.conf 可永久修改（sysctl -p /etc/sysctl.conf或者重启系统生效）
cat /proc/sys/fs/file-max

# 系统进程最大句柄数
# echo 123456 > /proc/sys/fs/nr_open 可临时修改
# fs.nr_open = 123456 添加在/etc/sysctl.conf 可永久修改（sysctl -p /etc/sysctl.conf或者重启系统生效）
cat /proc/sys/fs/nr_open

# 系统单用户最大句柄数
# ulimit -n 123456 可临时修改
# * soft nofile 65535 || * hard nofile 65535 2行添加在/etc/security/limits.conf 可永久修改（重新登录用户生效）
ulimit -n
ulimit -a 可以查看所有信息

# 结论
# 1、容器句柄数受宿主机进程句柄数限制（容器内部可以单独调整句柄数限制，但是只能低于宿主机设置的进程句柄数限制） /proc/sys/fs/nr_open
# 2、所有容器受宿主机最大句柄数限制，加起来不能超过宿主机最大句柄数限制（有些容器有例外，比如mysql容器）  /proc/sys/fs/file-max
```
