## <font color='red'> 2.1 Traffic Management </font>
Istio’s traffic routing rules let you easily control the flow of traffic and API calls between services. Istio simplifies configuration of service-level properties like circuit breakers, timeouts, and retries, and makes it easy to set up important tasks like A/B testing, canary rollouts, and staged rollouts with percentage-based traffic splits. It also provides out-of-box failure recovery features that help make your application more robust against failures of dependent services or the network.

Istio’s traffic management model relies on the Envoy proxies that are deployed along with your services. All traffic that your mesh services send and receive (data plane traffic) is proxied through Envoy, making it easy to direct and control traffic around your mesh without making any changes to your services.

**This is a repeat of the previous Lab to setup your environment.  If you have successfully completed the previous Lab then start with Lab 01 Fault Tolerance**


---

#### <font color='red'>IMPORTANT: ONLY REQUIRED IF YOUR INSTALLATION HAS FAILED </font> 
<strong>Please ensure you start with a clean environment. 
If you have previously run minikube, you will need to delete the existing instance.

You do not need to do these steps if you have successfully completed the previous Labs - Deploy Istio + Bookinfo + Gateway </strong>

stop an existing minikube instance:
```
minikube stop
```
to delete  minikube:
```
minikube delete
```
to start minikube:
```
minikube start
```
check minikube status:
```
minikube status
```
in a new terminal start the loadbalancer:
```
minikube tunnel
```

---

### <font color='red'> Deploy Istio </font>
in a new terminal download istio (latest): 
```
curl -L https://istio.io/downloadIstio | sh -
```
add the istioctl client to your path (follow instructions in download):
```
$ export PATH=$PATH:$HOME/.istioctl/bin
```
check istio:
```
istioctl x precheck
```
install istio default configuration:
```
deploy Istio 'default' profile:
```
istioctl install --set profile=default
```
check istio:
```
kubectl -n istio-system get deploy
```
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