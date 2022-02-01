
## 通过istioctl方式安装
1. 下载istio安装文件
```
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.11.5 TARGET_ARCH=x86_64 sh -
cd istio-1.11.5
export PATH=$PWD/bin:$PATH
```

2. 通过istiocli安装profile
``` 
./bin/istioctl install --set profile=default -y
```

3.检查安装是否成功
```
kubectl get pods -A
```


## 删除所有istio组件
```
istioctl x uninstall --purge
```