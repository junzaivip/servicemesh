
部署目标规则和v1版本的虚拟服务
```
kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml
kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml
```

灰度发布
```
k apply -f virtual-service-reviews-50-v3.yaml
```