## <font color="orange"> 2.3 Blue / Green Deployment </font>
The blue-green deployment approach does this by ensuring you have two production environments, as identical as possible. At any time one of them, let's say blue for the example, is live. As you prepare a new release of your software you do your final stage of testing in the green environment.  

Once the software is working in the green environment, you switch the router so that all incoming requests go to the green environment - the blue one is now idle.

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
port forward:  
```
kubectl port-forward -n istio-system svc/istio-ingressgateway 6324:80 
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