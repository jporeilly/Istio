## <font color='red'> 2.1 Traffic Management </font>
Istio’s traffic routing rules let you easily control the flow of traffic and API calls between services. Istio simplifies configuration of service-level properties like circuit breakers, timeouts, and retries, and makes it easy to set up important tasks like A/B testing, canary rollouts, and staged rollouts with percentage-based traffic splits. It also provides out-of-box failure recovery features that help make your application more robust against failures of dependent services or the network.

Istio’s traffic management model relies on the Envoy proxies that are deployed along with your services. All traffic that your mesh services send and receive (data plane traffic) is proxied through Envoy, making it easy to direct and control traffic around your mesh without making any changes to your services.

---
## <font color='red'> Delete Istio & Reset Minikube </font>

> Adapted from the [Istio Quick Start](https://istio.io/docs/setup/kubernetes/quick-start/) 

<font color="red"> You only need to delete Istio and Minikube if you wish to start with a clean deployment. </font>

first delete the Istio namespace
```
kubectl delete ns istio-system
```
the namesapce can also be deleted using K8s ext. in VS..

ensure minikube is up and running:
```
minikube start
```
in anew terminal start the loadbalancer:
```
minikube tunnel
```
in a new terminal check everything:
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

### <font color='red'> Deploy Istio </font>
check whats running on Kubernetes:
```
kubectl get all
```

in a new terminal download istio (latest): 
```
curl -L https://istio.io/downloadIstio | sh -
```
in a new terminal download istio (course version 1.7.4):
```
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.7.4 TARGET_ARCH=x86_64 sh -
```
add the istioctl client to your path:
```
export PATH="$PATH:/home/foundry/Istio-1.X.X/bin"
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
deploy Istio 'default' profile:
```
istioctl install --set profile=default
```
check istio:
```
kubectl -n istio-system get deploy
```
---

### <font color='red'> Verify Istio </font>
check running objects:
```
kubectl get pods,svc
```
istio has been deployed to its own namespace istio-system:
```
kubectl get pods -n istio-system
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
check label:
```
kubectl describe ns default
```
---

### <font color='red'> Check what's running </font>
check everything:
kubectl - no app installed / running:
```
kubectl get pods,svc
```
---

### <font color='red'> Deploy BookInfo</font>
deploy the bookinfo app:
```
kubectl apply -f 00_bookinfo.yaml
```

check PODs & services:
```
kubectl get pods,svc
```
---

### <font color='red'> Deploy Ingress Gateway </font>
deploy gateway:
```
kubectl apply -f 00_bookinfo-istio-gateway.yaml
```
---

### <font color='red'> 1.2.3 Verify the Istio-Ingress Gateway </font>
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
10.x.x.x with IP address

> check http://localhost/productpage
---