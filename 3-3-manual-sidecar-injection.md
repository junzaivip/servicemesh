## 手工注入istio sidecar

1. 生成一个K8S deployment示例
```
cat << EOF > nginx-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 1 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
EOF

```

2. 在default ns部署nginx示例
```
kubectl apply -f nginx-deployment.yml
```


3. 执行手工注入命令
```
kubectl delete -f nginx-deployment.yml
istioctl kube-inject -f nginx-deployment.yml | kubectl apply -f -
```


## 自动注入

```
kubectl label namespace default istio-injection=enabled
#disable 自动注入
kubectl label namespace default istio-injection-
```