## <font color='red'> 2.5 Circut Breaker </font>
The basic idea behind the circuit breaker is very simple. You wrap a protected function call in a circuit breaker object, which monitors for failures.   
Once the failures reach a certain threshold, the circuit breaker trips, and all further calls to the circuit breaker return with an error, without the protected call being made at all. 

---
Ensure you have reset the virtualservices + destination rules.

in a new terminal download istio (latest): 
```
curl -L https://istio.io/downloadIstio | sh -
```
install istio - default profile:
```
istioctl install --set profile=default
```
enable auto proxy injection:
```
kubectl label namespace default istio-injection=enabled
```
deploy bookinfo app:
```
kubectl apply -f 00_bookinfo-v1.yaml
```
deploy bookinfo bookinfo-gateway:
```
kubectl apply -f 00_bookinfo-gateway.yaml
```
---

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

deploy virtualservices:
```
kubectl apply -f 00_all-virtualservices-v1.yaml
```
deploy destination rules:
```
kubectl apply -f 00_all-destination-rules-v1.yaml
```
> check http://localhost/productpage  
---