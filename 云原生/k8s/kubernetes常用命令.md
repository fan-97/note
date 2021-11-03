

---



- 给节点打上标签

```shell
kubectl label nodes <node_name> key1=val1 key2=val2

eg:
# kubectl label nodes 10.16.17.180 zk-node=true
```

- 删除某个节点的标签

```shell
kubectl label nodes <node_name> key1-

eg:
# kubectl label nodes 10.16.17.180 zk-node-
```

- 查询节点的标签

```shell
kubectl get node --show-labels=true
```