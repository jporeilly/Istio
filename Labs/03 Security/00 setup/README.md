### <font color="orange"> Deploy Istio - Module 03 Secure with mTLS </font>
Mutual TLS (mTLS) authentication ensures that traffic is both secure and trusted in both directions between a client and server. It allows requests that do not log in with an identity provider (like IoT devices) to demonstrate that they can reach a given resource. Client certificate authentication is also a second layer of security for team members who both log in with an identity provider (IdP) and present a valid client certificate.  

With a root certificate authority (CA) in place, Access only allows requests from devices with a corresponding client certificate. When a request reaches the application, Access responds with a request for the client to present a certificate. If the device fails to present the certificate, the request is not allowed to proceed. If the client does have a certificate, Access completes a key exchange to verify.

---
 install istio default configuration (no mTLS):
```
 istioctl install --set profile=default
```
---

### <font color="orange"> Configure auto proxy injection </font>
anything that gets deployed to the default namespace will have Istio proxy - Envoy - automatically injected: 
```
kubectl label namespace default istio-injection=enabled
```
---

### <font color="orange"> Deploy BookInfo App </font>
deploy the bookinfo app v1:
```
kubectl apply -f 00_bookinfo.yaml
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
> browse to http://localhost/productpage

---

### <font color="orange"> Deploy Legacy App </font>
deploy legacy app:  
this app doesn't run through Istio (injection disabled):
```
kubectl apply -f 00_sleep.yaml
```
check thats its up and running:
```
kubectl get pods -n sleep 
```
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
---