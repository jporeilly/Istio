## <font color="orange"> 4.1 Visualizing the Service Mesh </font>

Using Kiali to graph telementry bewteen the services.

### 1.0 Pre-reqs



### <font color="orange"> 4.1.1 Publish the Kiali UI </font>
check Kiali service:

> check http://localhost:20001/kiali/console/

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

## 4.1.3 Generate some load
use Fortio to send a few hundred requests to the app:
```
docker container run `
  --add-host "bookinfo.local:192.168.2.119" `
  fortio/fortio `
  load -c 32 -qps 25 -t 30s http://bookinfo.local/productpage
```
- Back to Kiali _Graph_