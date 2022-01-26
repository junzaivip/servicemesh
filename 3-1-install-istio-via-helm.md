
###通过Helm方式安装Istio

1. 安装Helm（已经在本地安装过的可以跳过此步骤）
```
# use version 3.8.0 as an example
wget  https://get.helm.sh/helm-v3.8.0-rc.2-linux-amd64.tar.gz
tar zxf  helm-v3.8.0-rc.2-linux-amd64.tar.gz
mv linux-amd64/helm   /usr/local/bin/
```

2. 为Helm配置istio repo
```
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update
```

3. 创建 istio-system 命名空间
```
kubectl create namespace istio-system
```

4. 安装istio所需要的基础资源
```
helm install istio-base istio/base -n istio-system
```

5. 安装istiod
```
helm install istiod istio/istiod -n istio-system --wait
```

6. 安装istio ingress（入口）
```
kubectl create namespace istio-ingress  # 创建命名空间
kubectl label namespace istio-ingress istio-injection=enabled  #开启自动注入
helm install istio-ingress istio/gateway -n istio-ingress --wait  # 安装ingress
```

## 通过helm删除istio安装环境
1. 显示已安装组件
```
helm ls -n istio-system
NAME       NAMESPACE    REVISION UPDATED         STATUS   CHART        APP VERSION
istio-base istio-system 1        ... ... ... ... deployed base-1.0.0   1.0.0
istiod     istio-system 1        ... ... ... ... deployed istiod-1.0.0 1.0.0
```

2. 删除istio ingress gateway（如果已安装）
```
helm delete istio-ingress -n istio-ingress
kubectl delete namespace istio-ingress
```

3. 删除istiod组件
```
helm delete istiod -n istio-system
```

4. 删除istio base资源
```
helm delete istio-base -n istio-system
```

5. 删除istio namespace
```
kubectl delete namespace istio-system
```