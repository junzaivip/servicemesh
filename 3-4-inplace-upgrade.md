# 原地升级

### 假设已经安装了istio 1.11.x

## 下载安装istio 1.12.1
```
istioctl proxy-status
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.12.1 TARGET_ARCH=x86_64 sh -
cd istio-1.12.1
./bin/istioctl x precheck
./bin/istioctl upgrade
```

control plane 已经更新，不过data plane需要手动更新
```
istioctl proxy-status
kubectl rollout restart deployment --namespace default
istioctl proxy-status
```