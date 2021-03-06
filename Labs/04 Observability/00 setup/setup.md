## <font color='red'> 4.1 Observability </font>
Istio generates detailed telemetry for all service communications within a mesh. This telemetry provides observability of service behavior, empowering operators to troubleshoot, maintain, and optimize their applications – without imposing any additional burdens on service developers. Through Istio, operators gain a thorough understanding of how monitored services are interacting, both with other services and with the Istio components themselves.

Istio generates the following types of telemetry in order to provide overall service mesh observability:

- Metrics Istio generates a set of service metrics based on the four “golden signals” of monitoring (latency, traffic, errors, and saturation). Istio also provides detailed metrics for the mesh control plane. A default set of mesh monitoring dashboards built on top of these metrics is also provided.
- Distributed Traces. Istio generates distributed trace spans for each service, providing operators with a detailed understanding of call flows and service dependencies within a mesh.
- Access Logs. As traffic flows into a service within a mesh, Istio can generate a full record of each request, including source and destination metadata. This information enables operators to audit service behavior down to the individual workload instance level.

---
### <font color='red'> Installing Istio </font>
ensure minikube is up and running:
```
minikube start
```
check minikube status:
```
minikube status
```
in a new terminal start the loadbalancer:
```
minikube tunnel
```
---

### <font color='red'> Deploy Istio </font>
check whats running on Kubernetes:
```
kubectl get all
```
in a new terminal download istio (latest): 
```
curl -L https://istio.io/downloadIstio | sh -
```
add the istioctl client to your path (follow instructions in download):
```
export PATH="$PATH:/home/Istio/Istio-1.X.X/bin" (replace with Istio version)
```
check istio:
```
istioctl x precheck
```

install istio default configuration:
list istio profiles:
```
istioctl profile list
```
deploy Istio 'default' profile:
```
istioctl install --set profile=default
```
check istio:
```
kubectl -n istio-system get deploy
```
---

### <font color='red'> Verify Istio </font>
check docker is up and running:
```
systemctl status docker
```
check running objects:
```
kubectl get pods,svc
```

---

### <font color='red'> Configure auto proxy injection </font>
in a terminal check namespaces:
```
istioctl analyze --all-namespaces
```
anything that gets deployed to the default namespace will have Istio proxy - Envoy - automatically injected: 
```
kubectl label namespace default istio-injection=enabled
```
check label:
```
kubectl describe ns default
```

---

### <font color='red'> Check what's running </font>
check everything:
kubectl - no app installed / running:
docker info - look at the number of istio containers running:
```
kubectl get pods,svc
```
let's check Docker:  
notice the number of Containers:
```
docker info
```
---

### <font color='red'> Deploy BookInfo</font>
deploy the bookinfo app:
```
kubectl apply -f 00_bookinfo-v1.yaml
```
check PODs & services:
```
kubectl get pods,svc
```
---

### <font color='red'> Deploy Ingress Gateway </font>
deploy gateway:
```
kubectl apply -f 00_bookinfo-gateway.yaml
```
---

### <font color='red'> Verify the Istio-Ingress Gateway </font>
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

> check http://bookinfo.local/productpage

---


### <font color='red'> Deploy Visualization Tools </font>
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
in the istioctl terminal:
access kiali dashboard:
```
istioctl dashboard kiali
````

> check: http://localhost:20001/kiali/console/istio

in the istioctl terminal:
access grafana dashboard:
```
istioctl dashboard grafana
````

> check: http://localhost:3000/

---

### <font color='red'> Troubleshooting </font>
check istio deployment:
```
istioctl verify-install
```
to delete istio:
```
kubectl delete namespace istio-system
```

to clean up minikube:
delete from kubeconfig:
```
minikube stop
```
```
minikube delete
```

For kubectl commands: https://kubernetes.io/docs/reference/kubectl/cheatsheet/

---