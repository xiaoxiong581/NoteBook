##### 查询集群有哪些资源类型（Kind）
`kubectl api-resources`
##### 查询集群节点
`kubectl get node`
##### 查询集群namespace
`kubectl get namespace`
##### 查询集群所有service
`kubectl --all-namespaces get svc`
##### 查询集群所有pod
`kubectl --all-namespaces get pod`
##### 查询指定namespace下所有pod
`kubectl -n {namespace name} get pod -o wide`
##### 进入指定pod容器
`kubectl -n {namespace} exec -it {pod name} bash`
##### 进入指定pod中某个容器（适合pod中含有多个容器）
`kubectl -n {namespace} exec -it {pod name} -c {container name} bash`
##### 编辑指定service信息
`kubectl -n {namespace} edit svc {service name}`
##### 查看pod详情
`kubectl -n {namespace} describe pod {pod name}`
##### 通过api-server获取pod详情
`curl https://127.0.0.1:8080/api/v1/namespaces/{namespace}/pods/{podName} --header "Authorization: Bearer {token}" -k`
