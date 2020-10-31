## <font color="orange"> Installing Istio </font>

> Adapted from the [Istio Quick Start](https://istio.io/docs/setup/kubernetes/quick-start/)

---

### <font color="orange"> 1.1.1 Deploy Istio </font>

check whats running on Kubernetes:
```
kubectl get all
```
clear the screen:
```
cls
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
clear the screen:
```
cls
```
---

### <font color="orange"> 1.1.2 Verify Istio </font>

check running objects:
```
kubectl get pods,svc
```
istio has been deployed to its own namespace istio-system:
```
kubectl get pods -n istio-system
```
> all components have memory requests

clear the screen:
```
cls
```
---

### <font color="orange"> 1.1.3 Configure auto proxy injection </font>

check namespaces:
```
istioctl analyze --all-namespaces
```
anything that gets deployed to the default namespace will have Istio proxy - Envoy - automatically injected: 
```
kubectl label namespace default istio-injection=enabled
```
check label:
```
kubectl describe -n default
```
clear the screen:
```
cls
```
---

### <font color="orange"> 1.1.4 Check what's running </font>

check everything:
kubectl - no app installed / running:
docker info - look at the number of istio containers running:
```
kubectl get pods,svc
```
let's check Docker:  
notice the number of Containers:
```
docker info
```
clear the screen:
```
cls
```
---