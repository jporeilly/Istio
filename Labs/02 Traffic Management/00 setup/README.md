## <font color="orange"> Deploy Istio - Module 02 Traffic Management </font>
Istio’s traffic routing rules let you easily control the flow of traffic and API calls between services. Istio simplifies configuration of service-level properties like circuit breakers, timeouts, and retries, and makes it easy to set up important tasks like A/B testing, canary rollouts, and staged rollouts with percentage-based traffic splits. It also provides out-of-box failure recovery features that help make your application more robust against failures of dependent services or the network.

Istio’s traffic management model relies on the Envoy proxies that are deployed along with your services. All traffic that your mesh services send and receive (data plane traffic) is proxied through Envoy, making it easy to direct and control traffic around your mesh without making any changes to your services.

---
### <font color="orange"> Install Istio - Demo Profile </font>
install istio demo configuration:
```
istioctl install --set profile=demo
```
---

### <font color="orange"> Configure auto proxy injection </font>
anything that gets deployed to the default namespace will have Istio proxy - Envoy - automatically injected: 
```
kubectl label namespace default istio-injection=enabled
```
---

### <font color="orange"> Deploy BookInfo + Istio Gateway </font>
deploy the bookinfo app:
```
kubectl apply -f 00_bookinfo.yaml
```
check PODs:
```
kubectl get pods
```
check services:
```
kubectl get svc
```
---

### <font color="orange"> Deploy Gateway </font>
deploy gateway:
```
kubectl apply -f 00_bookinfo-gateway.yaml
```
> check http://localhost/productpage

port forward:  
```
kubectl port-forward -n istio-system svc/istio-ingressgateway 6324:80
```
> check http://localhost:6324/productpage
---