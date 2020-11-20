## <font color='red'> 1.1 Installing Istio </font>

> Adapted from the [Istio Quick Start](https://istio.io/docs/setup/kubernetes/quick-start/)  

ensure minikube is up and running:
```
minikube start
```

check minikube status:
```
minikube status
```
view addons:
```
minikube addons list
```

in a new terminal access dashboard:
```
minikube dashboard
```

in anew terminal start the loadbalancer:
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
in a new terminal download istio (version 1.7.4):
```
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.7.4 TARGET_ARCH=x86_64 sh -
```
add the istioctl client to your path:
```
export PATH="$PATH:/home/foundry/Istio-1.7.4/bin"
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
istioctl install --set profile=demo
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
to upgrade deployment:
```
istioctl upgrade
```
add the istioctl client to your path:
```
export PATH="$PATH:/home/foundry/Istio-1.7.4/bin"
```
reload variables:
```
source ~/.bash_profile
```
check istio:
```
istioctl x precheck
```


to delete istio:
```
kubectl delete namespace istio-system
```

to clean up minikube:
```
minikube stop
```
```
minikube delete
```