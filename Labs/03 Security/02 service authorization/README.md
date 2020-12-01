## <font color='red'> 3.2 Service Authorization </font>
Kubernetes authorizes API requests using the API server. It evaluates all of the request attributes against all policies and allows or denies the request.  

All parts of an API request must be allowed by some policy in order to proceed. This means that permissions are denied by default.
Istio Authorization Policy enables access control on workloads in the mesh.  

To configure an authorization policy, you create an AuthorizationPolicy custom resource. An authorization policy includes a selector, an action, and a list of rules.

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
kubectl delete pa -n default default-namespace-permissive

kubectl delete pa -n default details-service-strict
```
---

### <font color='red'> 3.2.1 DENY authorization to all Services </font>

apply a deny-all authorization policy for all services:
```
kubectl apply -f 01_deny-all.yaml
```
> check http://localhost/productpage  

RBAC: access denied - not authorized.  

---

### <font color='red'>3.2.2 ALLOW access to Productpage Service </font>

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

### <font color='red'> 3.2.3 Try from Unauthorized Service </font>
ensure were using the docker daemon from within minikube:
```
eval $(minikube docker-env)
```
run a shell in the reviews container:
```
docker container ls --filter name=k8s_istio-proxy_details
```
```
docker container exec -it $(docker container ls --filter name=k8s_istio-proxy_details --format '{{ .ID}}') sh
```
try accessing the reviews & ratings services:
```
curl http://reviews:9080/1
```
```
curl http://ratings:9080/ratings/1
```
they request will be RBAC denied as not authorized from the details service. 
---

clean up:
you can also delete the policies from the K8s extension in VS.

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
---