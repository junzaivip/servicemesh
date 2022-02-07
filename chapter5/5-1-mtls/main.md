```
k create ns test
k label ns test istio-injection-
k apply -f  sleep.yaml -n test 

设置对等认证 - permissive
cat <<EOF | kubectl apply -f -
apiVersion: "security.istio.io/v1beta1"
kind: "PeerAuthentication"
metadata:
  name: "default"
  namespace: "default"
spec:
  mtls:
    mode: PERMISSIVE
EOF

测试
k exec -it sleep-557747455f-bfsjn  -n test  -c sleep  -- curl http://httpbin.default:8000/headers

强制模式
##  permissive -> strict
cat <<EOF | kubectl apply -f -
apiVersion: "security.istio.io/v1beta1"
kind: "PeerAuthentication"
metadata:
  name: "default"
  namespace: "default"
spec:
  mtls:
    mode: STRICT
EOF


测试
k exec -it sleep-557747455f-cvwgm  -n test  -c sleep  -- curl http://httpbin.default:8000/headers

## istio服务注入
kubectl apply -f <(istioctl kube-inject -f sleep.yaml) -n test

### 获得新的pod id
k exec -it sleep-54f874965c-d6rct  -n test  -c sleep  -- curl http://httpbin.default:8000/headers

## 删除pod
kubectl delete -f <(istioctl kube-inject -f sleep.yaml) -n test
