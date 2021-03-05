## <font color='red'> 2.2 Dark Launch </font>  
Dark launching is the process of releasing production-ready features to a subset of your users prior to a full release. This enables you to decouple deployment from release, get real user feedback, test for bugs, and assess infrastructure performance.



---

#### <font color='red'>IMPORTANT: </font> 
<strong>Please ensure you start with a clean environment. 
If you have previously run minikube, you will need to delete the existing instance.</strong>

It is recommended to start with a clean minikube environment  

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
kubectl apply -f 00_bookinfo-v1.yaml
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

### <font color='red'> Verify the Istio-Ingress Gateway </font>
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


### <font color='red'> Verify the Istio-Ingress Gateway </font>


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