```
#switch to ns default

kubectl apply -f httpbin-v1.yaml
kubectl apply -f httpbin-v2.yaml
kubectl apply -f svc.yaml
kubectl apply -f vs.yaml
kubectl delete deployment httpbin


kubectl logs -f httpbin-v1-75d9447d79-r8k8q  
kubectl logs -f httpbin-v2-fb86d8d46-xj4zp

## k get sidecar
## remove sidecar restriction in the test namespace: delete the trafficoutbound policy
## k edit sidecar default -n test
## make sure the mtls is not enabled at this stage
## k edit PeerAuthentication  default


cat <<EOF | istioctl kube-inject -f - | kubectl create -f -
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
      containers:
      - name: sleep
        image: curlimages/curl
        command: ["/bin/sleep","3650d"]
        imagePullPolicy: IfNotPresent
EOF



kubectl exec -it sleep-7854b8bc5-tr52k   -- curl -I http://httpbin.default:8000/ip 
kubectl apply -f vs-mirror.yaml
kubectl exec -it sleep-7854b8bc5-tr52k   -- curl -I http://httpbin.default:8000/ip 

```
