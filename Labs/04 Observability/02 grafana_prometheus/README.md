## <font color="orange"> 4.2 Monitoring with Prometheus & Grafana </font>
Observe the metrics coming into Prometheus and the Istio dashboards in Grafana.

---

### <font color="orange"> 4.2.2 Publish the Prometheus UI </font>
check prometheus service:
````
kubectl -n istio-system get svc prometheus
````

in a new terminal:
```
istioctl dashboard prometheus
```
> browse to http://localhost:9090
- Select `istio_requests_total`
- Switch to _Graph_
- Check _Status_/_Targets_ - Kubernetes service discovery

Generate some load- send requests for next 30 minutes:
```
docker container run `
  --add-host "bookinfo.local:192.168.2.119" `
  fortio/fortio `
  load -c 32 -qps 25 -t 30m http://bookinfo.local/productpage
```
- Back to _Graph_ view in Prometheus

---

### <font color="orange"> 4.2.2 Publish the Grafana UI </font>
check grafana service:
````
kubectl -n istio-system get svc grafana
````
> in a new terminal:
```
istioctl dashboard grafana
```
> browse to http://localhost:3000
 - _Istio Mesh Dashboard_ - overview
 - _Istio Service Dashboard_ - drill down into service 

---

### <font color="orange"> 4.2.3 Deploy a failing service </font>
it may be worth deleting everything and starting with a clean deployment:

update the reviews-v2 service to add `503` faults:
```
kubectl apply -f 03_reviews-v2-abort.yaml
```
> check Grafana - Istio Service Dashboard > reviews.default.svc.cluster.local

> check Kiali - notice the traffic between productpage-v1 & v2 and reviews