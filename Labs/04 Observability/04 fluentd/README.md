## <font color='red'> 4.4 Logging from Istio and Envoy </font>
Use the EFK stack - Elasticsearch, Fluentd and Kibana to record and search logs.

---

### <font color='red'> 4.4.1 Deploy the Logging Stack </font>
Deployments, services etc. in Elasticsearch, Kibana and Fluentd:
list the addons in minikube:
```
minikube addons list
```
deploy efk on minikube:
```
minikube addons enable efk
```
check Pods in namespace kube-system:
```
kubectl get pods,svc -n kube-system
```
ensure the logs are forwarded to elasticsearch:
```
kubectl logs 
```
enter Cluster-IP address for Kibana service:  

> browse to Kibana at http://10.x.x.x:5601

or port forward:
```
kubectl -n kube-system port-forward $(kubectl -n kube-system get pod -l app=kibana -o jsonpath='{.items[0].metadata.name}') 5601:5601 &amp;
```

- In _Discover_ create index pattern for `logstash*`

---