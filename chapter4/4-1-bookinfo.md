
## 安装bookinfo 示例
```
export istio_path=<where you download the istio installation package>
kubectl label namespace default istio-injection=enabled
kubectl apply -f $istio_path/samples/bookinfo/platform/kube/bookinfo.yaml
kubectl get svc -A
kubectl get pods -A
```

## 安装ingress gateway
```
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
kubectl get svc -A
```

## 通过public IP查看bookinfo服务
![ingress gateway IP](./images/ingressgateway-IP.png)