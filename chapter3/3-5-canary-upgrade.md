```
## 
假设已经安装了其他版本的istio
# curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.11.5 TARGET_ARCH=x86_64 sh -
cd ../istio-1.11.5
./bin/istioctl x precheck
./bin/istioctl install --set revision=1-11-5
kubectl get pods -n istio-system -l app=istiod
istioctl proxy-status

去除istio-injection=enabled label
## kubectl label --overwrite  namespace default  istio.io/rev=1-11-5 istio-injection-

# 如果没有istio-injection label可以使用这个命令
kubectl label --overwrite  namespace default  istio.io/rev=1-11-5
kubectl rollout restart deployment -n default
```