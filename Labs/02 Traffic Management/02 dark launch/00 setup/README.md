## <font color='red'> 2.2 Dark Launch </font>  
Dark launching is the process of releasing production-ready features to a subset of your users prior to a full release. This enables you to decouple deployment from release, get real user feedback, test for bugs, and assess infrastructure performance.

---
Ensure you have reset the virtualservices + destination rules.

Just reviews-v1 service.

install istio:
```
istioctl install --set profile=demo
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
 > check http://localhost/productpage  
 

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