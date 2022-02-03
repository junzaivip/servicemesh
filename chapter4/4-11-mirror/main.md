```
kubectl apply -f httpbin-v1.yaml
kubectl apply -f httpbin-v2.yaml
kubectl apply -f svc.yaml
kubectl apply -f vs.yaml

kubectl logs -f httpbin-v1-75d9447d79-r8k8q  
kubectl logs -f httpbin-v2-fb86d8d46-xj4zp
kubectl exec -it -n test sleep-7854b8bc5-tr52k   -- curl -I http://httpbin.default:8000/ip 
kubectl apply -f vs-mirror.yaml
kubectl exec -it -n test sleep-7854b8bc5-tr52k   -- curl -I http://httpbin.default:8000/ip 

```