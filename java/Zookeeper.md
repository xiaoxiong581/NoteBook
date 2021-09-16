##### 1. 设置acl digest
```sh
addauth digest test:testccc
setAcl /test auth:test:testccc:cdrwa
#查看是否设置成功
getAcl /test
```

##### 2. 删除acl digest
```sh
setAcl /test world:anyone:cdrwa
```
