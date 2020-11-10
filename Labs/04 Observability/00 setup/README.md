## <font color="orange"> 4.1 Observability </font>
in a new terminal download istio: 
```
curl -L https://istio.io/downloadIstio | sh -
```
add the istioctl client to your path:demo
```
export PATH="$PATH:/home/foundry/Istio-1.7.4/bin"
```
check istio:
```
istioctl x precheck
```

troubleshooting:
```
istioctl upgrade
```
restart istio services:
```
kubectl rollout restart deployment
```

install istio demo configuration:
list istio profiles:
```
istioctl profile list
```
deploy Istio 'default' profile:
```
istioctl install --set profile=demo
```
check istio:
```
kubectl -n istio-system get deploy
```
---

### <font color="orange"> Configure auto proxy injection </font>
anything that gets deployed to the default namespace will have Istio proxy - Envoy - automatically injected: 
in a terminal check namespaces:
```
istioctl analyze --all-namespaces
```
```
kubectl label namespace default istio-injection=enabled
```
---

### <font color="orange"> Deploy BookInfo App </font>
deploy the bookinfo app v1:
```
kubectl apply -f 00_bookinfo-v1.yaml
```
check PODs & services:
```
kubectl get pods,svc
```
---

### <font color="orange"> Deploy Gateway </font>
deploy gateway:
```
kubectl apply -f 00_bookinfo-gateway.yaml
```

check gateway:
```
kubectl get gateway
```
```
kubectl get svc istio-ingressgateway -n istio-system
```
notice the EXTERNAL-IP 10.x.x.x  this will have to be mapped to localhost in etc/hosts
```
sudo nano /etc/hosts
```
replace the existing IP with current IP address:
> check http://localhost/productpage
---

### <font color="orange"> Deploy Kiali </font>
install prometheus:  
````
kubectl apply -f 00_prometheus.yaml
````
check prometheus service:
````
kubectl -n istio-system get svc prometheus
````
install kiali:
````
kubectl apply -f 00_kiali.yaml
````
check kiali service:
````
kubectl -n istio-system get svc kiali
````
install grafana:  
````
kubectl apply -f 00_grafana.yaml
````
install zipkin:
````
kubectl apply -f 00_zipkin.yaml
````
install jaeger:  
````
kubectl apply -f 00_jaeger.yaml
````
check jaeger service:
````
kubectl -n istio-system get svc tracing
````
access kiali dashboard:
```
istioctl dashboard kiali
````
````
> check: http://localhost:20001/kiali/console/istio
````
---