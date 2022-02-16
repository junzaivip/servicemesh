
部署目标规则和v1版本的虚拟服务
```
# export istio_path=<where you download the istio installation package>

kubectl apply -f $istio_path/samples/bookinfo/networking/virtual-service-all-v1.yaml
kubectl apply -f $istio_path/samples/bookinfo/networking/destination-rule-all.yaml
```

灰度发布
```
k apply -f virtual-service-reviews-50-v3.yaml
```