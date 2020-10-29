## <font color="orange"> Istio Labs </font>

> Adapted from the [Istio Quick Start](https://istio.io/docs/setup/kubernetes/quick-start/)

## Welcome to the Istio course ..
Istio is a fundamental component of Lumada Data Ops - Foundry. It’s an open source service mesh   
platform that provides control over your microservices and as you'll see .. enables Developers to  
perform Dark Launches, Blue / Green, Canary and Circut Breaker deployments as part of the DevOps cycle.

The self-paced course is aimed at DevOps folks. 

The course is composed of 5 Modules:  

  01 Deploy Istio + Bookinfo App  
  02 Traffic Managemnt  
  03 Secure with mTLS  
  04 Observability  
  05 Istio in Production

> Each lab has a README.md file which outlines the commands.

---

### <font color="orange"> Module 01 - Deploy Istio + Bookinfo App </font>

### <font color="teal"> Lab: 01 Deploy Istio </font>
Install Istio - 'demo' profile.
Envoy proxy.

### <font color="teal"> Lab: 02 Bookinfo App </font>
Deploy the Bookinfo App.
Deploy Ingress Gateway.

---

### <font color="orange"> Module 02 - Traffic Management </font>

### <font color="teal"> Lab: 00 Deploy Istio </font> 
Deploy Istio with auto sidecar injection. 

### <font color="teal"> Lab: 01 Fault Tolerance </font>
VirtualServices and Destination Rules.  
Fault - Details page.  
Timeout - Details page.  
Retry - Details page.  

### <font color="teal"> Lab: 02 Dark Launch </font>
Deploy Reviews:v2 to a number of users. 

### <font color="teal"> Lab: 03 Blue / Green Launch </font>
Deploy Productpage:v2 on separate test environment.
bookinfo.local - original
test.bookinfo.local - test

### <font color="teal"> Lab: 04 Canary Launch </font>
Deploy Productpage:v2 to bookinfo.local environment.
traffic shaping

### <font color="teal"> Lab: 05 Circut Breaker </font>
Deploy Details:v2 and 4 replicas.
Outlier detection enables unhealthy POds to be terminated.

---

### <font color="orange"> Module 03 - Secure with mTLS </font>

### <font color="teal"> Lab: 00 Deploy Default Istio </font>
Deploy Istio with auto sidecar injection. 

### <font color="teal"> Lab: 01 No Auth </font>
Trusted network - hack network. 

### <font color="teal"> Lab: 02 mutualTLS </font>
Authentication with mTLS. 

### <font color="teal"> Lab: 03 Service Auth </font>
Deploy Productpage:v2

### <font color="teal"> Lab: 04 JWT Auth </font>
Deploy Productpage:v2

---

### <font color="orange"> Module 04 - Observability </font>

### <font color="teal"> Lab: 00 Deploy Istio </font>
Deploy Istio with auto sidecar injection. 

### <font color="teal"> Lab: 01 Kiali </font>
Authentication with mTLS. 

### <font color="teal"> Lab: 02 Grafana & Prometheus </font>
Deploy Productpage:v2

### <font color="teal"> Lab: 03 Jaegar </font>
Deploy Productpage:v2

### <font color="teal"> Lab: 04 FluentD </font>
Deploy Productpage:v2

---

### <font color="orange"> Module 05 - Istio in Production </font>

### <font color="teal"> Lab: 00 Deploy Istio </font>
Deploy Istio with auto sidecar injection. 

### <font color="teal"> Lab: 01 Configure & Deploy </font>
Authentication with mTLS. 

### <font color="teal"> Lab: 02 Migrate to production </font>
Deploy Productpage:v2

---