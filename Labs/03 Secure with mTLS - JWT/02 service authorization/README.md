### <font color='orange'> 4.2 Service Authorization </font>
Istio Authorization Policy enables access control on workloads in the mesh.

#### <font color='orange'> 4.2.1 Restrict access to all Services </font>
apply a deny-all authorization policy for all service:
```
kubectl apply -f 01_deny-all.yaml
```

> check http://localhost/productpage  
> check http://localhost:6324/productpage  

RBAC: access denied - authenticated with mTLS but not authorized.  

#### <font color='orange'>4.2.2 Allow access to productpage </font>
apply the updated authorization policy to allow access to productpage service:
```
kubectl apply -f 02_allow-productpage.yaml
```
> check http://localhost/productpage  
> check http://localhost:6324/productpage

#### <font color='orange'>4.2.3 Allow access to details and reviews service </font>
apply the updated authorization policy to allow access to details & reviews service:
```
kubectl apply -f 03_allow-details-reviews.yaml
```
> check http://localhost/productpage  
> check http://localhost:6324/productpage

#### <font color='orange'> 4.2.4 Try from unauthorized service </font>
run a shell in the reviews container:
```
docker container ls --filter name=k8s_reviews
```
```
docker container exec -it $(docker container ls --filter name=k8s_reviews --format '{{ .ID}}') sh
```
try accessing the reviews & ratings APIs:
```
curl http://productpage:9080/1
```
```
curl http://ratings:9080/ratings/1
```
