# kubectl命令

```bash
kubectl [command] [TYPE] [NAME] [flags]
```

command: 操作类型，如create， get， describe, delete等

TYPE: 指定操作的资源类型，如pods

NAME: 指定资源名称，如忽略则默认命名空间下的所有同类资源

flags:  命令行选型，如覆盖默认服务器地址，端口，输出样式等


# 集群状态

查看集群状态
```bash
kubectl cluster-info
```

显示kubectl命令行及kube服务端的版本
```bash
kubectl version
```

显示支持的API版本集合
```bash
kubectl api-version
```

显示当前kubectl配置
```bash
kubectl config view
```

查看集群中节点
```bash
kubectl get no
```

查看部署
```bash
kubectl get deploy
```

查看replicaSet
```bash
kubectl get repilcaset
```

查看pod
```bash
kubectl get po
```

查看service
```bash
kubectl get svc
```

查看endport
```bash
kubectl get ep
```

# 创建新资源

使用yaml文件创建

```bash
kubectl create -f <res.yaml>
```

使用某镜像创建

```bash
kubectl run <name> --image=<image>
```


# 检查与调试

## 查看某种类型的资源

```bash
kubectl get <type> <name>
```

## 检查某特定资源实例

```bash
kubectl describe <type> <name>
```


## 检查某POD的日志（标准输出）
```bash
kubectl logs <某pod的name>
```

在容器中执行命令
```bash
kubectl exec
```

# 部署管理

实现水平扩展或收缩
```bash
kubectl scale
```

查看变更部署到哪一步了
```bash
kubectl rollout status
```

部署的历史
```bash
kubectl rollout history
```

回滚部署到最近 或者  回滚部署到某个特定版本
```bash
kubectl rollout undo
```

# 一个例子

## 查看集群状态

检查集群中节点状态和kube的功能集

查看集群中节点
```bash
kubectl get no
```

查看部署
```bash
kubectl get deploy
```

查看replicaSet
```bash
kubectl get repilcaset
```

查看pod
```bash
kubectl get po
```

查看日志
```bash
kubectl log 
```

查看service
```bash
kubectl get svc
```

## 创建nginx的Deploy

部署一个nginx的单实例

```bash
kubectl run nginx --image=nginx:1.7.9
```

nginx是我们为该实例取的名字

--image 指定我们使用哪一个镜像

## 检查Deploy

查看该部署的运行状况

```bash
# 简单信息
kubectl get deploy nginx
# 详细信息
kubectl describe deploy nginx
```

## 服务发现

如何从外部访问

### 第一种方式：直接暴露端口

```bash
kubectl expose deploy nginx --type=NodePort --name=nginx-ext --port=80
```

--name 指定了服务名，这里取nginx-ext是为了与第二种方式建立的nginx区分

### 第二种方式：利用yaml文件

nginx.svc.yaml

```
apiVersion: v1
kind: Service
metadata:
	name: nginx
	labels:
		app: nginx
spec:
	ports:
	- name: http
	  port: 8888
	  nodePort: 30001
	  targetPort: 80
	selector:
	  run: nginx
	type: NodePort
```

创建了一个服务，服务名为nginx，外界端口为30001，内部端口为8888


## 部署管理

实例的水平扩展 或 收缩

```bash
# 将pod个数扩展到3
kubectl scale deploy nginx --replicas=3
kubectl get rs
kubectl get po
kubectl get ep
```

实例的滚动升级

```bash
# 将nginx有1.7.9升级为1.9.1
kubectl set image deploy nginx nginx=nginx:1.9.1
# 查看rollout状态
kubectl rollout status deploy nginx
# 查看rollout的历史
kubectl rollout history deploy nginx
```

实例的回滚

```bash
kubectl rollout undo deploy nginx
```

## 删除资源

释放不再需要的资源
```bash
kubectl delete svc nginx
```




























































