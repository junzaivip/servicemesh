## 设置sidecar流量拦截

1. 新建namespace
```
kubectl create ns test
```

2. 生成一个K8S deployment示例
```
cat << EOF > test.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: sleep
---
apiVersion: v1
kind: Service
metadata:
  name: sleep
  labels:
    app: sleep
    service: sleep
spec:
  ports:
  - port: 80
    name: http
  selector:
    app: sleep
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sleep
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sleep
  template:
    metadata:
      labels:
        app: sleep
    spec:
      terminationGracePeriodSeconds: 0
      serviceAccountName: sleep
      containers:
      - name: sleep
        image: curlimages/curl
        command: ["/bin/sleep", "3650d"]
        imagePullPolicy: IfNotPresent
        volumeMounts:
        - mountPath: /etc/sleep/tls
          name: secret-volume
      volumes:
      - name: secret-volume
        secret:
          secretName: sleep-secret
          optional: true
---
EOF

```

3. 执行手工注入命令
```
istioctl kube-inject -f test.yaml | kubectl apply -n test -f -
```

4. 访问kiali服务

```
kubectl get pods -n test
istioctl proxy-config clusters sleep-557747455f-c4j6c  -n test
##检查outbound traffic policy： 空表示 allow-all
k edit  sidecar default -n test

k exec -it -n test sleep-557747455f-c4j6c  -- curl kiali.istio-system.svc.cluster.local:20001
```

5. 设置sidecar资源进行流量拦截
```
cat << EOF > sidecar.yaml
apiVersion: networking.istio.io/v1beta1
kind: Sidecar
metadata:
  name: default
  namespace: test
spec:
  egress:
  - hosts:
    - "./*"
  outboundTrafficPolicy:
    mode: REGISTRY_ONLY
EOF
```

6. 创建sidecar资源，再次访问

```
kubectl apply -f sidecar.yaml -n test
k exec -it -n test sleep-557747455f-c4j6c  -- curl kiali.istio-system.svc.cluster.local:20001
istioctl proxy-config clusters sleep-557747455f-c4j6c

k edit sidecar default -n test
```
