## <font color='red'> 1.2 Deploying Bookinfo App </font>

> modified version of the [Istio BookInfo sample app](https://github.com/istio/istio/tree/master/samples/bookinfo)
---

### <font color='red'> 1.2.1 Deploy BookInfo</font>
deploy the bookinfo app:
```
kubectl apply -f 01_bookinfo.yaml
```

check PODs & services:
```
kubectl get pods,svc
```

if you need to redeploy app:
```
kubectl rollout restart deployment --namespace default
```

---

### <font color='red'> 1.2.2 Deploy Ingress Gateway </font>
deploy gateway:
```
kubectl apply -f 02_bookinfo-istio-gateway.yaml
```
---

### <font color='red'> 1.2.3 Verify the Istio-Ingress Gateway </font>
check PODs:
```
kubectl get pods
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
replace the 127.0.0.1 with IP address

> check http://localhost/productpage
---

### <font color='red'> 1.2.4 Kiali + Prometheus + Grafana </font>
Lets have a look at the telementry between the services:

install prometheus:  
````
kubectl apply -f 05_prometheus.yaml
````
check prometheus service:
````
kubectl -n istio-system get svc prometheus
````
install kiali:
````
kubectl apply -f 05_kiali.yaml
````
check kiali service:
make a note of the IP:
````
kubectl -n istio-system get svc kiali
````
install jaeger:  
````
kubectl apply -f 05_jaeger.yaml
````
check jaeger service:
````
kubectl -n istio-system get svc tracing
````
install grafana:
````
kubectl apply -f 05_grafana.yaml
````
check grafana service:
make a note of the IP:
````
kubectl -n istio-system get svc grafana
````
in the istioctl terminal:  

access kiali dashboard:
```
istioctl dashboard kiali
````
> check: http://localhost:20001/kiali/console

access grafana dashboard:
> http://10.x.x.x:3000/dashboard/db/istio-service-dashboard
---