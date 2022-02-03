# 创建 istio egressgateway
```
kubectl create ns istio-egress
kubectl label ns istio-egress istio-injection=enabled
helm install istio-egressgateway istio/gateway -n istio-egress  --wait
```

# 查看test namespace的sidecar流量拦截设置,并尝试访问httpbin.org
```
kubectl edit sidecar default -n test
kubectl exec -it -n test sleep-7854b8bc5-tr52k   -- curl -I httpbin.org/ip
```

# 创建egress gateway，服务入口和虚拟服务
```
 kubectl apply -f gateway.yaml
 kubectl apply -f serviceentry.yaml
 kubectl apply -f vs.yaml
```

# 再次访问httpbin.org
```
kubectl exec -it -n test sleep-7854b8bc5-tr52k   -- curl -I httpbin.org/ip
```