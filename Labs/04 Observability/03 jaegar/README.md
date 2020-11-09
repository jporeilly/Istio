## <font color="orange"> 4.3 Distributed Tracing </font>
Using Jaegar to visualize trace spans and investigate outliers.

---

### <font color="orange"> 4.3.1 Publish the Jaegar UI </font>
in a new terminal:
```
istioctl jaegar dashboard
```
> browse to: http://localhost:15032 

- Select service `productpage.default`
- Follow traces - OK & failing
- Zoom into timeline & check tags

---

### <font color="orange"> 4.3.2 Add service latency </font>
update the productpage to add 10s delays:
```
kubectl apply -f 02_productpage-delay.yaml
```
> check http://bookinfo.local/productpage & refresh

generate some load:

```
docker container run `
  --add-host "bookinfo.local:192.168.2.119" `
  fortio/fortio `
  load -c 32 -qps 25 -t 30s http://bookinfo.local/productpage
```
- Back to Jaegar
- Follow trace for outlier

---