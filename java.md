目录  
1. [Kafka](java/kafka.md)
1. [SpringBoot](java/springboot.md)


##### 生成证书文件
`keytool -genkey -alias customerManager -keyalg RSA -keysize 2048  -keystore server.keystore -validity 365`

##### nginx证书文件
`openssl req -new -x509 -nodes -out server.crt -keyout server.key`