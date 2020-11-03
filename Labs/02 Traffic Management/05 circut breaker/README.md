## <font color="orange"> 2.5 Circut Breaker </font>
The basic idea behind the circuit breaker is very simple. You wrap a protected function call in a circuit breaker object, which monitors for failures.   
Once the failures reach a certain threshold, the circuit breaker trips, and all further calls to the circuit breaker return with an error, without the protected call being made at all. 

#### <font color='red'> Ensure you have reset the the cluster. </font>

---
#### <font color="orange"> 2.5.1  Details Service v2 </font>
deploy details service v2 with 4 replicas:
```
kubectl apply -f 01_details-v2.yaml
```
check PODs:
```
kubectl get pods
```
check deployment:
```
kubectl describe svc,vs,dr details
```

> check http://localhost/productpage & refresh lots. Around 50% of details call fail.  

check logs:
```
kubectl logs -l app=details,version=v2 -c details
```
---

#### <font color="orange"> 2.5.2 Apply Circuit Breaker </font>
Outlier Detection is an Istio Resiliency strategy to detect unusual host behavior and evict the unhealthy hosts from the set of load balanced healthy hosts inside a cluster. It automatically tracks the status of each individual host and checks metrics like consecutive errors and latency associated with service calls. If it finds outliers, it will automatically evict them.

deploy updated rules with outlier detection:
```
kubectl apply -f 02_details-circuit-breaker.yaml
```
check deployment:
```
kubectl describe dr details
```

> check http://localhost/productpage & refresh lots. As pods return errors they get excluded - after a while there are no errors, requests only go to healthy pods.
---