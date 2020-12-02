## <font color='red'> 3.1 Secure with mTLS </font>
Mutual TLS (mTLS) authentication ensures that traffic is both secure and trusted in both directions between a client and server. It allows requests that do not log in with an identity provider (like IoT devices) to demonstrate that they can reach a given resource. Client certificate authentication is also a second layer of security for team members who both log in with an identity provider (IdP) and present a valid client certificate.  

With a root certificate authority (CA) in place, Access only allows requests from devices with a corresponding client certificate. When a request reaches the application, Access responds with a request for the client to present a certificate. If the device fails to present the certificate, the request is not allowed to proceed. If the client does have a certificate, Access completes a key exchange to verify.

---

### <font color='red'> Delete Istio & Reset Minikube </font>

> Adapted from the [Istio Quick Start](https://istio.io/docs/setup/kubernetes/quick-start/) 

<font color="red"> You will need to delete Istio and Minikube and deploy Istio with profile=default. </font>

install istio default configuration (no mTLS):  
check whats running on Kubernetes:
```
kubectl get all
```

in a new terminal download istio (latest): 
```
curl -L https://istio.io/downloadIstio | sh -
```
add the istioctl client to your path (follow instructions in terminal):
```
export PATH="$PATH:/home/foundry/Istio-1.X.X/bin"
```
check istio:
```
istioctl x precheck
```

install istio demo configuration:
in a new terminal list istio profiles:
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

### <font color='red'> Configure auto proxy injection </font>
in a terminal check namespaces:
```
istioctl analyze --all-namespaces
```
anything that gets deployed to the default namespace will have Istio proxy - Envoy - automatically injected: 
```
kubectl label namespace default istio-injection=enabled
```
---

### <font color='red'> Deploy BookInfo App </font>
deploy the bookinfo app v1:
```
kubectl apply -f 00_bookinfo.yaml
```
check PODs & services:
```
kubectl get pods,svc
```
---

### <font color='red'> Deploy Gateway </font>
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

### <font color='red'> Deploy Legacy App </font>
deploy legacy app:  
this app runs in 'sleep' namespace (injection disabled):
```
kubectl apply -f 00_sleep.yaml
```
check thats its up and running:
```
kubectl get pods -n sleep 
```
---

### <font color='red'> Deploy Kiali </font>
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
> check: http://localhost:20001/kiali/console

---