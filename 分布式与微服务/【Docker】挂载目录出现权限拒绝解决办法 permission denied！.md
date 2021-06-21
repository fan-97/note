使用docker命令运行一个容器并挂载目录到宿主机时，出现 permission denied！
## 发生条件
docker中运行minio镜像
```markdown
docker run -p 9090:9000 --name minio \ 
-v /mydata/minio/data:/data \
-v /mydata/minio/config:/root/.minio \
-d minio/minio server /data
```


​

​

## 出现问题：
ERROR Unable to initialize backend: mkdir /data/.minio.sys: permission denied
​

## 解决办法
通过添加启动参数 --privileged=true
​

```markdown
docker run -p 9090:9000 --name minio \
-v /mydata/minio/data:/data 
-v /mydata/minio/config:/root/.minio 
--privileged=true 
-d minio/minio server /dat
```
​

> 


