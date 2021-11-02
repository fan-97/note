

1. 利用awk 命令快速启动所有docker容器
> docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)

​

