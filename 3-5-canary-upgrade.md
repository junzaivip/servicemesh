### 灰度升/降级

# 下载istio 1.11.5
```
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.11.5 TARGET_ARCH=x86_64 sh -
cd istio-1.11.5
```

# 安装其他版本
```
./bin/istioctl x precheck
./bin/istioctl install --set revision=1-11-5
```
# 检查安装情况
```
kubectl get pods -n istio-system -l app=istiod
istioctl proxy-status
```

# 删除原先的自动注入label
```
kubectl label namespace default istio-injection- 
kubectl edit ns default # 查看label情况
kubectl rollout restart deployment -n default
```