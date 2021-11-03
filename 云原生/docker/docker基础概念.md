# 数据共享与持久化

docker中管理数据的两种方式：

- 数据卷（Data Volumes）
- 挂载主机目录（Bind mounts)

## 数据卷

`数据卷`是一个可以让一个或者多个容器共同使用的特殊目录，绕过 `UFS`。Feture:

- 可以在多个容器间共享和重用
- 对 `数据卷` 的修改会立马生效

- 对 `数据卷` 的更新，不会影响镜像
- 容器被删除，数据卷不会被删除

可以使用命令 `--mount` 或者 `-v` 来将数据卷挂载到容器内



创建一个数据卷

```sh
docker volume create my-vol	
```

查看所有的数据卷

```sh
docker volume ls 
----
DRIVER    VOLUME NAME
local     my-vol
```

查看数据卷的详细信息

```sh
docker volume inspect my-vol
---
[
    {
        "CreatedAt": "2021-10-21T23:13:00-07:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/my-vol/_data",
        "Name": "my-vol",
        "Options": {},
        "Scope": "local"
    }
]
```

用 `docker run`命令的时候，使用 `--mount` 来将数据卷挂载到容器中

```sh
docker run -d -P \
--name webserver3 \
--mount source=my-vol,target=/var/log/nginx/ \
nginx
```

使用 `-v` 的方式

```sh
docker run -d -P \
--name webserver3 \
-v my-vol:/var/log/nginx/ \
nginx
```

## 挂载主机目录

挂载一个主机目录作为数据卷：使用 --mount 标记可以指定挂载一个本地主机的目录到容器中去。

```plain
$ docker run -d -P \
    --name web \
    # -v /src/webapp:/opt/webapp \
    --mount type=bind,source=/src/webapp,target=/opt/webapp \
    training/webapp \
    python app.py
```

上面的命令加载主机的 /src/webapp 目录到容器的 /opt/webapp目录。这个功能在进行测试的时候十分方便，比如用户可以放置一些程序到本地目录中，来查看容器是否正常工作。本地目录的路径必须是绝对路径，以前使用 -v 参数时如果本地目录不存在 Docker 会自动为你创建一个文件夹，现在使用 --mount 参数时如果本地目录不存在，Docker 会报错。

Docker 挂载主机目录的默认权限是 读写，用户也可以通过增加`readOnly`指定为 只读。



```sh
$ docker run -d -P \
    --name web \
    # -v /src/webapp:/opt/webapp:ro \
    --mount type=bind,source=/src/webapp,target=/opt/webapp,readonly \
    training/webapp \
    python app.py
```

加了readonly之后，就挂载为 只读 了。如果你在容器内 /opt/webapp 目录新建文件，会显示如下错误:

```sh
/opt/webapp # touch new.txt
touch: new.txt: Read-only file system
```

查看数据卷的具体信息：在主机里使用以下命令可以查看 web 容器的信息

```sh
$ docker inspect web
...
"Mounts": [
    {
        "Type": "bind",
        "Source": "/src/webapp",
        "Destination": "/opt/webapp",
        "Mode": "",
        "RW": true,
        "Propagation": "rprivate"
    }
],
```

挂载一个本地主机文件作为数据卷：--mount标记也可以从主机挂载单个文件到容器中

```sh
$ docker run --rm -it \
   # -v $HOME/.bash_history:/root/.bash_history \
   --mount type=bind,source=$HOME/.bash_history,target=/root/.bash_history \
   ubuntu:17.10 \
   bash

root@2affd44b4667:/# history
1  ls
2  diskutil list
```