## <font color="orange"> 2.1 Traffic Management </font>
Istio’s traffic routing rules let you easily control the flow of traffic and API calls between services. Istio simplifies configuration of service-level properties like circuit breakers, timeouts, and retries, and makes it easy to set up important tasks like A/B testing, canary rollouts, and staged rollouts with percentage-based traffic splits. It also provides out-of-box failure recovery features that help make your application more robust against failures of dependent services or the network.

Istio’s traffic management model relies on the Envoy proxies that are deployed along with your services. All traffic that your mesh services send and receive (data plane traffic) is proxied through Envoy, making it easy to direct and control traffic around your mesh without making any changes to your services.



---
## <font color="orange"> Minikube </font>

> Adapted from the [Istio Quick Start](https://istio.io/docs/setup/kubernetes/quick-start/)  
first delete the Istio namespace
```
kubectl delete ns istio-system
```

ensure minikube is up and running:
```
minikube start
```
start the loadbalancer:
```
minikube tunnel
```
check everything:
```
minikube dashboard
```

to clean up:
```
minikube stop
```
```
minikube delete
```
---

### <font color="orange"> Deploy Istio </font>
check whats running on Kubernetes:
```
kubectl get all
```

in a terminal download istio: 
```
curl -L https://istio.io/downloadIstio | sh -
```
add the istioctl client to your path:
```
export PATH="$PATH:/home/foundry/Istio-1.7.4/bin"
```
check istio:
```
istioctl x precheck
```

install istio demo configuration:
list istio profiles:
```
istioctl profile list
```
deploy Istio 'demo' profile:
```
istioctl install --set profile=demo
```
check istio:
```
kubectl -n istio-system get deploy
```
---

### <font color="orange"> Verify Istio </font>
check running objects:
```
kubectl get pods,svc
```
istio has been deployed to its own namespace istio-system:
```
kubectl get pods -n istio-system
```
---

### <font color="orange"> Configure auto proxy injection </font>
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

### <font color="orange"> Check what's running </font>
check everything:
kubectl - no app installed / running:
```
kubectl get pods,svc
```
---

### <font color="orange"> Deploy BookInfo</font>
deploy the bookinfo app:
```
kubectl apply -f 00_bookinfo.yaml
```

check PODs & services:
```
kubectl get pods,svc
```
---

### <font color="orange"> Deploy Ingress Gateway </font>
deploy gateway:
```
kubectl apply -f 00_bookinfo-istio-gateway.yaml
```
---

### <font color="orange"> 1.2.3 Verify the Istio-Ingress Gateway </font>
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