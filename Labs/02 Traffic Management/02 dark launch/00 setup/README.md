## <font color='red'> 2.2 Dark Launch </font>  
Dark launching is the process of releasing production-ready features to a subset of your users prior to a full release. This enables you to decouple deployment from release, get real user feedback, test for bugs, and assess infrastructure performance.

---
Ensure you have reset the virtualservices + destination rules.

Just reviews-v1 service.

install istio:
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