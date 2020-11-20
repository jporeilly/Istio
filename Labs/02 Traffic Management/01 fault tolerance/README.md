## <font color='red'>  2.1 Istio and Fault-Tolerance </font>

Traffic management in Istio is governed by 2 important concepts:
- virtualservice  
defines a set of traffic routing rules to apply when a host is addressed. Each routing rule defines matching criteria for traffic of a specific protocol. If the traffic is matched, then it is sent to a named destination service (or subset/version of it) defined in the registry.

- destination rules  
defines policies that apply to traffic intended for a service after routing has occurred. These rules specify configuration for load balancing, connection pool size from the sidecar, and outlier detection settings to detect and evict unhealthy hosts from the load balancing pool.

---

### <font color='red'> 2.1.1 Route traffic through Istio </font>
deploy a v1-virtualservice for each service:  
configured Istio to route to the v1 version of the Bookinfo microservices, most importantly the reviews service v1:
```
kubectl apply -f 01_all-virtualservices-v1.yaml
```
set destination rules:  
```
kubectl apply -f 01_all-destination-rule-v1.yaml
```
view the destination rules::
```
kubectl get destinationrules -o yaml
```
> check http://localhost/productpage (same functionality - just v1)  
---

### <font color='red'> 2.1.2 Reviews Service - Timeout </font>
deploy reviews v2 virtualservice:  

 > productpage → reviews:v2 → ratings:v1  
```
kubectl apply -f 02_reviews-v2-virtualservice.yaml
```
deploy reviews v2 destination rule - black stars: 
```
kubectl apply -f 02_reviews-v2-destination-rule.yaml
```
> check http://localhost/productpage - reviews-v2.  

lets add a 2sec delay to the ratings service:  
```
kubectl apply -f 02_ratings-virtualservice-fault-delay-2s.yaml
```
> check http://localhost/productpage - 2s delay.  

route the requests to reviews v2 with 0.5 sec timeout:  
```
kubectl apply -f 02_reviews-virtualservice-fault-timeout-0.5s.yaml
```
> check http://localhost/productpage  
fails as the request to the review page timeouts after 0.5sec which is less than the 2 sec delay to the ratings service.
---