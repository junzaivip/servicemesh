## 安装压测工具

```
kubectl apply -f samples/httpbin/sample-client/fortio-deploy.yaml
FORTIO_POD=$(kubectl get pod|grep fortio | awk '{print $1 }')
kubectl exec  -it $FORTIO_POD -c fortio -- /usr/bin/fortio load -curl http://httpbin:8000/get
kubectl exec -it $FORTIO_POD  -c fortio -- /usr/bin/fortio load -c 2 -qps 0 -n 20 -loglevel Warning http://httpbin:8000/get
kubectl exec -it $FORTIO_POD  -c fortio -- /usr/bin/fortio load -c 3 -qps 0 -n 30 -loglevel Warning http://httpbin:8000/get

kubectl exec $FORTIO_POD -c istio-proxy -- pilot-agent request GET stats | grep httpbin | grep pending

```
熔断触发，upstream_rq_pending_overflow计数器+1
