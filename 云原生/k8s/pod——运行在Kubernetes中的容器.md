



# Label（标签）

可以用来组织pod，也可以组织其他Kubenetes资源。标签是可以附加到资源的任意键值对，用以选择具有该标签的资源

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: kubia-manual-v2
  labels:
    creation_method: manual      # 该pod拥有两个标签
    env: prod
spec:
  containers:
  - image: luksa/kubia
    name: kubia
    ports:
    - containerPort: 8080
      protocol: TCP
```

- 给node打上标签并且将pod调度到该node

```shell
$ kubectl label node xxx-node gpu=true
```

kubia-gpu.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: kubia-gpu
spec:
  nodeSelector:
    gpu: "true"     # 只将pod部署到包含标签 gpu=true的节点上
  containers:
  - image: luksa/kubia
    name: kubia
```

# 注解pod

注解也是键值对，注解不能对对象进行分组。注解可以为每个pod或其他API对象添加说明。

**注解的内容总共不超过256KB**

```shell
kubectl annotate pod kubia-manual mycompany.com/someannotation="foo bar"
```

