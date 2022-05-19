# :warning: 注意：目前此项目处于试用阶段

# Imc-operator
imc-operator项目用于以CRD+Controller的模式，支持使用腾讯云镜像缓存服务，而不需要使用云api接口直接调用。  
关于腾讯云镜像缓存，可以参考文档：https://cloud.tencent.com/document/product/457/65908

下面将会介绍如何在腾讯云eks集群中，部署imc-operator来使用腾讯云镜像缓存。

## 创建命名空间
```shell
kubectl create ns imc-operator-system
```

## 创建CRD
```shell
kubectl apply -f manifests/crds/imagecache.yaml
```

## 创建cm、secret配置项
:warning: 请先修改imc-operator-secret.yaml, 以base64的格式填入CLUSTER_ID、VPC_ID、REGION、SECRET_ID、SECRET_KEY的值。
其中SECRET_ID、SECRET_KEY是调用腾讯云api的密钥。
```shell
kubectl apply -f manifests/configmap/imc-operator-manager-config.yaml
kubectl apply -f manifests/secret/imc-operator-secret.yaml
```

## 创建RBAC相关
```shell
kubectl apply -f manifests/rbac/serviceaccount.yaml
kubectl apply -f manifests/rbac/clusterrole.yaml
kubectl apply -f manifests/rbac/clusterrolebinding.yaml 
```

## 部署controller
```shell
kubectl apply -f manifests/controllers/imc-operator-controller-manager.yaml
```

## 创建ImageCache示例
```shell
# 一个简单的示例
kubectl apply -f manifests/samples/eks_v1_imagecache.yaml
# 一个带有更多参数的示例
kubectl apply -f manifests/samples/eks_v1_imagecache_more_para.yaml
```

## 查看ImageCache
```shell
kubectl get imc
# 如有异常可查看events
kubectl describe imc xxx
```