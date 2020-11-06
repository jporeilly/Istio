## <font color="orange"> 4.2 Monitoring with Prometheus & Grafana </font>
Observe the metrics coming into Prometheus and the Istio dashboards in Grafana.

---

### <font color="orange"> 4.2.1 Publish the Prometheus UI </font>
in a new terminal:
```
istioctl dashboard prometheus
```
> browse to http://localhost:15030
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
> in a new terminal:
```
istioctl dashboard grafana
```
> browse to http://localhost:3000/dashboard/db/istio-mesh-dashboard
 - _Istio Mesh Dashboard_ - overview
 - _Istio Service Dashboard_ - drill down into service 

---

### <font color="orange"> 4.2.3 Deploy a failing service </font>
update the reviews-v2 service to add `503` faults:
```
kubectl apply -f 03_reviews-v2-abort.yaml
```
> check [Grafana](http://localhost:3000/dashboard/db/istio-mesh-dashboard?orgId=1&refresh=5s&from=now-5m&to=now&var-service=reviews.default.svc.cluster.local&var-srcns=All&var-srcwl=All&var-dstns=All&var-dstwl=All)

> check [Kiali](http://localhost:20001/kiali/console/graph/namespaces/?edges=requestsPercentage&graphType=versionedApp&namespaces=default&unusedNodes=false&injectServiceNodes=true&pi=10000&duration=300&layout=dagre)

---