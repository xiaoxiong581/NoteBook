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
