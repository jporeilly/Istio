### <font color='orange'> 3.2 Service Authorization </font>
Kubernetes authorizes API requests using the API server. It evaluates all of the request attributes against all policies and allows or denies the request.  

All parts of an API request must be allowed by some policy in order to proceed. This means that permissions are denied by default.
Istio Authorization Policy enables access control on workloads in the mesh.  

To configure an authorization policy, you create an AuthorizationPolicy custom resource. An authorization policy includes a selector, an action, and a list of rules.

![Istio - Authorization](./img/bookinfo-authorization.png)

> Watch a video: Istio Authorization (02:54):  

[![Istio Authorization](./img/lumada.png)](https://youtu.be/dg9SJrMh5o8 "istio authorization")

check peer athentication:
```
kubectl get pa --all-namespaces
```
check destination rules:
```
kubectl get dr --all-namespaces
```

<font color="red"> delete </font>any peer authentication policies and destination rules:  

delete peer athentication:
```
kubectl delete pa -n <namespace> <peerathentication>
```
---

#### <font color='orange'> 3.2.1 DENY authorization to all Services </font>

> Watch a video: Istio Authorization - DENY (02:54):  

[![Istio Authorization - DENY](./img/lumada.png)](https://youtu.be/dg9SJrMh5o8 "istio authorization - DENY")

apply a deny-all authorization policy for all services:
```
kubectl apply -f 01_deny-all.yaml
```

> check http://localhost/productpage  

RBAC: access denied - not authorized.  

> Watch a video: Istio Authorization - DENY (09:00):  

[![Istio Authorization - DENY ALL](./img/lumada.png)](https://youtu.be/j3Mz0LS5U2s "istio authorization")

---

#### <font color='orange'>3.2.2 ALLOW access to Services </font>

> Watch a video: Istio Authorization - ALLOW (09:00):  

[![Istio Authorization - DENY ALL](./img/lumada.png)](https://youtu.be/j3Mz0LS5U2s "istio authorization")

apply the updated authorization policy to allow access to productpage service:
```
kubectl apply -f 02_allow-productpage.yaml
```
> check http://localhost/productpage  

apply the updated authorization policy to allow access to details & reviews from productpage service:
```
kubectl apply -f 02_allow-details-reviews.yaml
```
> check http://localhost/productpage  

apply the updated authorization policy to allow access to ratings from reviews service:
```
kubectl apply -f 02_allow-reviews-ratings.yaml
```
> check http://localhost/productpage  
---

#### <font color='orange'> 3.2.3 Try from unauthorized service </font>
run a shell in the reviews container:
```
docker container ls --filter name=k8s_reviews
```
```
docker container exec -it $(docker container ls --filter name=k8s_reviews_reviews-v1 --format '{{ .ID}}') sh
```
try accessing the reviews & ratings APIs:
```
curl http://productpage:9080/1
```
```
curl http://ratings:9080/ratings/1
```

clean up:
```
kubectl delete authorizationpolicy.security.istio.io/deny-all
```
```
kubectl delete authorizationpolicy.security.istio.io/productpage-viewer
```
```
kubectl delete authorizationpolicy.security.istio.io/details-viewer
```
```
kubectl delete authorizationpolicy.security.istio.io/reviews-viewer
```
```
kubectl delete authorizationpolicy.security.istio.io/ratings-viewer
```