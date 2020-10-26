### <font color='orange'> 4.2 Service Authorization </font>
Kubernetes authorizes API requests using the API server. It evaluates all of the request attributes against all policies and allows or denies the request.  

All parts of an API request must be allowed by some policy in order to proceed. This means that permissions are denied by default.
Istio Authorization Policy enables access control on workloads in the mesh.

![Istio - Authorization](./img/bookinfo-authorization.png)

> Watch a video: Istio Authorization (09:00):  

[![Istio Authorization](./img/lumada.png)](https://youtu.be/j3Mz0LS5U2s "istio authorization")

check peer athentication:
```
kubectl get pa --all-namespaces
```
check destination rules:
```
kubectl get dr --all-namespaces
```

<font color="red"> delete </font>any peer authentication policies and destination rules:  

---

#### <font color='orange'> 4.2.1 DENY authorization to all Services </font>
apply a deny-all authorization policy for all services:
```
kubectl apply -f 01_deny-all.yaml
```

> check http://localhost/productpage  

RBAC: access denied - not authorized.  

> Watch a video: Istio Authorization - DENY (09:00):  

[![Istio Authorization - DENY ALL](./img/lumada.png)](https://youtu.be/j3Mz0LS5U2s "istio authorization")

---

#### <font color='orange'>4.2.2 ALLOW access to Services </font>
apply the updated authorization policy to allow access to productpage service:
```
kubectl apply -f 02_allow-productpage.yaml
```
> check http://localhost/productpage  

apply the updated authorization policy to allow access to details & reviews service:
```
kubectl apply -f 03_allow-details-reviews.yaml
```
> check http://localhost/productpage  

---

#### <font color='orange'> 4.2.3 Try from unauthorized service </font>
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
---