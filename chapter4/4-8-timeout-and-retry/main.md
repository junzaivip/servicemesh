### 1. 创建 vs指向 reviews v2版本
```
k apply -f 1-vs.yaml 
```

### 2. 创建 vs指向 ratings的v1版本，同时注入延时2s
```
k apply -f 2-vs-ratings.yaml 
```

### 3. 在reviews VS配置中设置超时时间为1s
```
k apply -f 3-timeout.yaml 

ratings的延时2s会导致reviews在1s超时后直接返回错误
```

### 4. 加入重试机制，访问ratings服务会尝试2次
```
k apply -f 4-retry.yaml
```

### 5. 开启日志
```
k apply -f 5-log.yaml
```

### 6. 查看日志
```
k logs -f  ratings-v1-5f9d88b968-lptjc  -c istio-proxy  -n default
```