## 1. 安装VirtualBox

下载软件源文件

```shell
sudo wget https://download.virtualbox.org/virtualbox/rpm/el/virtualbox.repo -P /etc/yum.repos.d
```

安装virtualBox5.2.x

```shell
sudo yum install VirtualBox-5.2
```

查看 服务状态

```sh
systemctl status vboxdrv


### printf
● vboxdrv.service - VirtualBox Linux kernel module
   Loaded: loaded (/usr/lib/virtualbox/vboxdrv.sh; enabled; vendor preset: disabled)
   Active: active (exited) since Wed 2021-11-03 14:18:04 CST; 45s ago
  Process: 11138 ExecStart=/usr/lib/virtualbox/vboxdrv.sh start (code=exited, status=0/SUCCESS)
    Tasks: 0
   Memory: 0B

Nov 03 14:18:04 VM-0-15-centos systemd[1]: Starting VirtualBox Linux kernel module...
Nov 03 14:18:04 VM-0-15-centos vboxdrv.sh[11138]: vboxdrv.sh: Starting VirtualBox services.
Nov 03 14:18:04 VM-0-15-centos systemd[1]: Started VirtualBox Linux kernel module.

```

## 2.安装Docker

## 3. 安装 kubectl

下载最新版本

```sh
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

下载指定版本

```sh
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.23.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```



## 4. 安装minikube

下载minikube 并安装

```sh
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

## 5.使用minikube安装k8s

```
minikube start --vm-dirver=none #要加这个参数，不加会报错
```

安装指定版本的kubernetes集群

```sh
# 查阅版本
minikube get-k8s-versions
# 选择版本启动
minikube start --kubernetes-version v1.7.3 --vm-driver=none
```

- minikube stop

  停止集群

- minikube status

​    查看集群运行状态

- minikube delete 

  删除集群

