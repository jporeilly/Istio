
### <font color="orange"> Deploy Istio </font>
install istio demo configuration:
```
istioctl install --set profile=demo
```
clear the screen:
```
cls
```
---

### <font color="orange"> Configure auto proxy injection </font>
anything that gets deployed to the default namespace will have Istio proxy - Envoy - automatically injected: 
```
kubectl label namespace default istio-injection=enabled
```
check label:
```
kubectl describe namespace default
```
clear the screen:
```
cls
```
---

### <font color="orange"> Deploy BookInfo + Istio Gateway </font>
Deploy the bookinfo app:
```
kubectl apply -f 00_bookinfo.yaml
```
clear the screen:
```
cls
```
check PODs:
```
kubectl get pods
```
check services:
```
kubectl get svc
```
clear the screen:
```
cls
```
---

### <font color="orange"> Deploy Gateway </font>
deploy gateway:
```
kubectl apply -f 00_bookinfo-gateway.yaml
```
clear the screen:
```
cls
````
---

### <font color="orange"> Verify the Gateway </font>
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
clear the screen:
```
cls
```
> browse to http://localhost/productpage

port forward:  
```
kubectl port-forward -n istio-system svc/istio-ingressgateway 6324:80
```
> browse to http://localhost:6324/productpage
---