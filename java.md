目录  
1. [Kafka](java/kafka.md)
1. [SpringBoot](java/springboot.md)


##### 生成证书文件
`keytool -genkey -alias customerManager -keyalg RSA -keysize 2048  -keystore server.keystore -validity 365`

##### nginx证书文件
`openssl genrsa -out server.key 2048`  
`openssl req -new -x509 -nodes -out server.crt -keyout server.key`

##### consul获取配置
```sh
wget --header "X-Consul-Token: {token}" http://127.0.0.1:8500/v1/kv/config/global_config
echo {value} | base64 -d
```
