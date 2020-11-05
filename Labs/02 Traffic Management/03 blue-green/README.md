### <font color="orange"> 2.3 Deploy Productpage-v2 with test Domain </font>
The blue-green deployment approach does this by ensuring you have two production environments, as identical as possible. At any time one of them, let's say blue for the example, is live. As you prepare a new release of your software you do your final stage of testing in the green environment.  

Once the software is working in the green environment, you switch the router so that all incoming requests go to the green environment - the blue one is now idle.  


add `bookinfo.local` and `test.bookinfo.local` domains to hosts file:
```
$ sudo nano /etc/hosts
```
---

### <font color="orange"> 2.3.1 deploy Productpage-v2 </font>  
use existing gateway:
```
kubectl describe gateway bookinfo-gateway
```
deploy productpage-v2 with test domain:
> productpage:v1 → bookinfo.local  
> productpage:v2 → test.bookinfo.local
```
kubectl apply -f 01_productpage-v2.yaml
```
check deployment:
```
kubectl get pods -l app=productpage
```
check bookinfo.local host/gateway virtualservice for productpage-v1:
```
kubectl describe vs bookinfo
```
check test.bookinfo.local host/gateway virtualservice for productpage-v2:
```
kubectl describe vs bookinfo-test
```
> live v1 http://bookinfo.local/productpage  
---

### <font color="orange"> 2.3.2 Blue/Green Deployment - Test to Live </font>
deploy test to live:
```
kubectl apply -f 02_productpage-test-to-live.yaml
```
check live deployment:
```
kubectl describe vs bookinfo
```
> live is now v2 http://bookinfo.local/productpage  

check test deployment:
```
kubectl describe vs bookinfo-test
```
> test is now v1 http://test.bookinfo.local/productpage  
---

### <font color="orange"> 2.3.3 Blue/Green deployment - Switch back </font>
deploy live to test:
```
kubectl apply -f 03_productpage-live-to-test.yaml
```
> live is back v1 http://bookinfo.local/productpage  

> test is back v2 http://test.bookinfo.local/productpage  
---