## <font color='red'> 4.4 Logging from Istio and Envoy </font>
Use the EFK stack - Elasticsearch, Fluentd and Kibana to record and search logs.

---

### <font color='red'> 4.4.1 Deploy the logging stack </font>
Deployments, services etc. in Elasticsearch, Kibana and Fluentd:

deploy efk on minikube
```
minikube -p efk start
```
check Pods in namespace kube-system:
```
kubectl get pods,svc -n kube-system
```
enter Cluster-IP address for Kibana service:  

> browse to Kibana at http://10.x.x.x:5601

or port forward:
```
kubectl -n kube-system port-forward $(kubectl -n kube-system get pod -l app=kibana -o jsonpath='{.items[0].metadata.name}') 5601:5601 &amp;
```

- In _Discover_ create index pattern for `logstash*`

generate some load unning for 30 seconds:
```
docker container run `
  --add-host "bookinfo.local:192.168.2.119" `
  fortio/fortio `
  load -c 32 -qps 25 -t 30s http://bookinfo.local/productpage
```

- Back to Kibana
- Filter on `kubernetes.labels.app` _is_ `productpage`

---

### <font color='red'> 4.4.2 Configure Istio to log to Fluentd </font>
deploy the Fluentd:
```
kubectl apply -f 02_fluentd-istio.yaml
```
generate load - run a 30s burst of requests:
```
docker container run `
  --add-host "bookinfo.local:192.168.2.119" `
  fortio/fortio `
  load -c 32 -qps 25 -t 30s http://bookinfo.local/productpage
```
> refresh Kibana at http://localhost:5601 

- Filter on `kubernetes.container.name` _is_ `istio-proxy`
- These are Envoy logs 

---