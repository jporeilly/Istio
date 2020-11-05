## <font color="orange"> 2.4 Canary Deployment </font>
Canary deployment is like blue-green, except it’s more risk-averse. Instead of switching from blue to green in one step, you use a phased approach.

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
kubectl apply -f 00_bookinfo-v1+v2.yaml
```
deploy bookinfo bookinfo-gateway:
```
kubectl apply -f 00_bookinfo-gateway.yaml
```
port forward:  
```
kubectl port-forward -n istio-system svc/istio-ingressgateway 6324:80 
```
 > check http://bookinfo.local/productpage  

deploy virtualservices:
```
kubectl apply -f 00_all-virtualservices-v1+v2.yaml
```
deploy destination rules:
```
kubectl apply -f 00_all-destination-rules-v1+v2.yaml
```
> check http://bookinfo.local/productpage  
---