## <font color="orange"> Deploying Bookinfo App </font>
> modified version of the [Istio BookInfo sample app](https://github.com/istio/istio/tree/master/samples/bookinfo)

---

### <font color="orange"> 1.2.1 Deploy BookInfo + Ingress Gateway</font>

deploy the bookinfo app:
```
kubectl apply -f 01_bookinfo.yaml
```

check PODs & services:
```
kubectl get pods,svc
```


show the productpage proxy setup:
```
kubectl describe pods -l app=productpage
```

find all proxy containers:
```
docker container ls --filter name=istio-proxy_*
```

check proxy processes for the product page:
```
docker container ls --filter name=istio-proxy_productpage* -q  
```
```
docker container top $(docker container ls --filter name=istio-proxy_productpage* -q)
```

---

### <font color="orange"> 1.2.2 Deploy Ingress Gateway </font>

deploy gateway:
```
kubectl apply -f 02_bookinfo-ingress-gateway.yaml
```

---

### <font color="orange"> 1.2.3 Verify the Ingress Gateway </font>

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

clear the screen (ensure PODs are up and running):
```
cls
````
> check http://localhost/productpage

---

### <font color="orange"> 1.2.4 Port Forward Gateway </font>

depending on your environment you may have to port forward requests. 

port forward:
````
kubectl port-forward -n istio-system svc/istio-ingressgateway 6324:80
````
> check http://localhost:6324/productpage
----

### <font color="orange"> 1.2.5 Kiali + Prometheus + Grafana </font>

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

access kiali dashboard:
```
istioctl dashboard kiali
````
access grafana dashboard:

> http://<IP>:3000/dashboard/db/istio-service-dashboard

---