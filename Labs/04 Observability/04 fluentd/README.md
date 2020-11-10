## <font color="orange"> 4.4 Logging from Istio and Envoy </font>
Use the EFK stack - Elasticsearch, Fluentd and Kibana to record and search logs.

---

### <font color="orange"> 4.4.1 Deploy the logging stack </font>
Deployments, services etc. in Elasticsearch, Kibana and Fluentd:
```
kubectl apply -f 01_logging_stack.yaml
```
check Pods in namespace logging:
```
kubectl get pods -n logging
```
> browse to Kibana at http://localhost:15033

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

### <font color="orange"> 4.4.2 Configure Istio to log to Fluentd </font>
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
> refresh Kibana at http://localhost:15033 

- Filter on `kubernetes.container.name` _is_ `istio-proxy`
- These are Envoy logs 

---