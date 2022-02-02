

1. 更新sidecar资源，只允许访问mesh内注册的服务
```
cat << EOF > sidecar.yaml
apiVersion: networking.istio.io/v1beta1
kind: Sidecar
metadata:
  name: block-test
  namespace: test
spec:
  egress:
  - hosts:
    - "./*"
  outboundTrafficPolicy:
    mode: REGISTRY_ONLY
EOF
```

2. 测试出现访问错误
```
k exec -it -n test sleep-7854b8bc5-cmmdq    -- curl -I  httpbin.org  
## should return 502 gateway error
```

3. 创建服务入口
```
cat << EOF > serviceentry.yaml
apiVersion: networking.istio.io/v1alpha3
kind: ServiceEntry
metadata:
  name: httpbin-ext
spec:
  hosts:
  - httpbin.org
  ports:
  - number: 80
    name: http
    protocol: HTTP
  resolution: DNS
  location: MESH_EXTERNAL
EOF
```

4. 再次访问成功
```
k exec -it -n test sleep-7854b8bc5-cmmdq    -- curl -I  httpbin.org
```
