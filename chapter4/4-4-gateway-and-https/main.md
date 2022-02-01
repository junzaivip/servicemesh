# 创建新的gateway并且连接到ingress gateway上
```
## 部署kiali服务
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.12/samples/addons/kiali.yaml

## 创建新的gateway并且暴露给mesh外访问
kubectl apply -f kiali-expose.yaml
```


# 生成证书并且加载到istio gateway
```
 openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -subj '/O=istio Inc./CN=istio.fun' -keyout private.key -out root.crt
 openssl req -out kiali.istio.fun.csr -newkey rsa:2048 -nodes -keyout kiali.istio.fun.key -subj "/CN=kiali.istio.fun/O=kiali"
 openssl x509 -req -days 365 -CA root.crt -CAkey private.key -set_serial 0 -in kiali.istio.fun.csr -out kiali.istio.fun.crt
 kubectl create -n istio-system secret tls https-kiali-credential --key=kiali.istio.fun.key --cert=kiali.istio.fun.crt
 kubectl get secret -n istio-system
 kubectl apply -f kiali-gw-https.yml
 curl  -k  https://kiali.istio.fun/ -I
 # Safiri 打开
```
