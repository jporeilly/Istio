## <font color='red'> 1.1 Installing Istio </font>

> Adapted from the [Istio Quick Start](https://istio.io/docs/setup/kubernetes/quick-start/)  

#### <font color='red'>IMPORTANT:</font> 
<strong>Please ensure you start with a clean environment. 
If you have previously run minikube, you will need to delete the existing instance.</strong>

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

### <font color='red'> 1.1.1 Deploy Istio </font>
check whats running on Kubernetes:
```
kubectl get all
```
in a new terminal download istio (latest): 
```
curl -L https://istio.io/downloadIstio | sh -
```
add the istioctl client to your path (follow instructions in download):
```
export PATH="$PATH:/home/Istio/Istio-1.X.X/bin" (replace with Istio version)
```
check istio:
```
istioctl x precheck
```

install istio default configuration:
list istio profiles:
```
istioctl profile list
```
deploy Istio 'demo' profile:
```
istioctl install --set profile=default
```
check istio:
```
kubectl -n istio-system get deploy
```
---

### <font color='red'> 1.1.2 Verify Istio </font>
check docker is up and running:
```
systemctl status docker
```
check running objects:
```
kubectl get pods,svc
```
istio has been deployed to its own namespace istio-system:
```
kubectl get pods -n istio-system
```
> all components have memory requests
---

### <font color='red'> 1.1.3 Configure auto proxy injection </font>
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

### <font color='red'> 1.1.4 Check what's running </font>
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
---

### <font color='red'> Troubleshooting </font>
check istio deployment:
```
istioctl verify-install
```
to delete istio:
```
kubectl delete namespace istio-system
```

to clean up minikube:
delete from kubeconfig:
```
minikube stop
```
```
minikube delete
```

For kubectl commands: https://kubernetes.io/docs/reference/kubectl/cheatsheet/