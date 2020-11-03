## <font color="orange"> 2.4 Canary Deployment </font>
Canary deployment is like blue-green, except it’s more risk-averse. Instead of switching from blue to green in one step, you use a phased approach.

---

### <font color="orange"> 2.4.1 Canary with 30% traffic to v2 </font>
deploy canary rules with 70/30 split:  
> productpage:v1 → bookinfo.local 70%  
> productpage:v2 → bookinfo.local 30%  

let's set the virtualservice:  
deploy productpage-v2:
```
kubectl apply -f 01_productpage-v2.yaml
```

> check http://bookinfo.local/productpage


```
kubectl apply -f 01_productpage-canary-70-30.yaml
```
check deployment on bookinfo.local:
```
kubectl describe vs bookinfo
```
> check http://bookinfo.local/productpage & refresh, mostly v1 responses with some v2  

check deployment on test.bookinfo.local:
```
kubectl describe vs bookinfo-test
```
> check http://test.bookinfo.local/productpage & refresh, just v2  

---

### <font color="orange"> 2.4.2 Canary Rollout </font>
shift traffic to v2:

- 55/45 split: kubectl apply -f 02_productpage-canary-55-45.yaml  
- 25/75 split: kubectl apply -f 02_productpage-canary-25-75.yaml  
- 0/100 split: kubectl apply -f 02_productpage-canary-0-100.yaml  

> check http://bookinfo.local/productpage & refresh  

---

### <font color="orange"> 2.4.3 Canary with Sticky Session </font>
deploy canary rules with 70/30 split: 
```
kubectl apply -f 01_productpage-canary-70-30.yaml
```
check deployment:
```
kubectl describe vs bookinfo
```
> check http://bookinfo.local/productpage & refresh, mostly v1 responses with some v2  

deploy canary split (70-30) with cookie:
```
kubectl apply -f 03_productpage-canary-with-cookie.yaml
```

> check http://bookinfo.local/productpage & refresh - once you hit v2 that puts a cookie in your browser, and you'll always get v2  

Check by disabling cookies:
_e.g. in Firefox_
- Options
- Search "cookies"
- Manage permissions
- Add `bookinfo.local` - with `Block`

> check http://bookinfo.local/productpage & refresh - back to 70/30 split  

---