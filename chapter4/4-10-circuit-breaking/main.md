## 安装压测工具

```
# export istio_path=<where you download the istio installation package>

kubectl apply -f $istio_path/samples/httpbin/sample-client/fortio-deploy.yaml -n default
FORTIO_POD=$(kubectl get pod -n default|grep fortio | awk '{print $1 }')
kubectl exec  -it $FORTIO_POD -n default -c fortio -- /usr/bin/fortio load -curl http://httpbin:8000/get

kubectl apply -f circuitbreaking.yaml  -n default

kubectl exec -it $FORTIO_POD  -n default  -c fortio -- /usr/bin/fortio load -c 2 -qps 0 -n 20 -loglevel Warning http://httpbin:8000/get
kubectl exec -it $FORTIO_POD  -n default -c fortio -- /usr/bin/fortio load -c 3 -qps 0 -n 30 -loglevel Warning http://httpbin:8000/get

kubectl exec $FORTIO_POD -c istio-proxy -n default -- pilot-agent request GET stats | grep httpbin | grep pending

```
熔断触发，upstream_rq_pending_overflow计数器+1


## clean up
```
k delete deployment fortio-deploy
k delete deployment nginx-deployment (if there is one)
```
