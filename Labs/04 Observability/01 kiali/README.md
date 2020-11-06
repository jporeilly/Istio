## <font color="orange"> 4.1 Visualizing the Service Mesh </font>

Using Kiali to graph telementry bewteen the services.
---

### <font color="orange"> Pre-reqs checklist </font>
* istio profile = default
* bookinfo-v1
* IP address of gateway

### <font color="orange"> 4.1.1 Publish the Kiali UI </font>
access Kiali UI:

> browse to http://localhost:20001/kiali/console/

- App graph in _Graph_
- Check `productpage` in Kiali _Workloads_

### <font color="orange"> 4.1.2 Canary Deployment </font>
deploy productpage-v2:
```
kubectl apply -f 01_productpage-v2-canary.yaml
```
> browse to http://bookinfo.local/productpage and refresh 

- Back to Kiali
- Switch versioned app graph
- Add _Requests percentage_ label
- Check bookinfo virtual service in _Istio Config_ (editable!)

deploy the other services:
```
kubectl apply -f 01_reviews-v2-canary.yaml
```
explore the various vKialio options...!

### <font color="orange"> 4.1.3 Generate some load </font>

kubectl get svc istio-ingressgateway -n istio-system

use Fortio to send a few hundred requests to the app:
```
docker run --add-host "bookinfo.local:192.168.145.53"  fortio/fortio load -c 32 -qps 25 -t 60s http://10.101.115.109/productpage
```
- back to Kiali _Graph_
---