## <font color='red'> 3.3 End-user Authorization </font>  
require end-user authentication with JWT and enforce access control based on the JWT claims.

---

### <font color='red'> 3.3.1 Decode the JWT </font>

The JWT is a base64 encoded string. 
Read the claims - select demo.jwt

- Issuer: `testing@secure.istio.io`
- Subject: `testing@secure.istio.io`
- Custom: `foo=bar`
---

### <font color='red'> 3.3.2 Require JWT  </font>
view the JWT:
```
eyJhbGciOiJSUzI1NiIsImtpZCI6IkRIRmJwb0lVcXJZOHQyenBBMnFYZkNtcjVWTzVaRXI0UnpIVV8tZW52dlEiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjQ2ODU5ODk3MDAsImZvbyI6ImJhciIsImlhdCI6MTUzMjM4OTcwMCwiaXNzIjoidGVzdGluZ0BzZWN1cmUuaXN0aW8uaW8iLCJzdWIiOiJ0ZXN0aW5nQHNlY3VyZS5pc3Rpby5pbyJ9.CfNnxWP2tcnR9q0vxyxweaF3ovQYHYZl82hAUsn21bwQd9zP7c-LS9qd_vpdLG4Tn1A15NxfCjp5f7QNBUo-KC9PJqYpgGbaXhaGx7bEdFWjcwv3nZzvc7M__ZpaCERdwU7igUmJqYGBYQ51vr2njU9ZimyKkfDe3axcyiBZde7G6dabliUosJvvKOPcKIWPccCgefSj_GNfwIip3-SsFdlR7BtbVUcqR-yv-XOxJ3Uc1MI0tz3uMiiZcyPV7sNCU4KRnemRIMHVOfuvHsU60_GhGbiSFzgPTAa9WTltbnarTbxudb_YEOx12JiwYToeX0DCPb43W1tzIBxgm8NxUg
```

apply the JWT authtentication policy for the productpage:
```
kubectl apply -f 02_productpage-authn-jwt.yaml
```
> check http://localhost/productpage -> `200`  

no JWT is passed in the request.  

verify that a request with an invalid JWT is denied:
```
Authorization: Bearer invalidToken  
```
you need to add an JWT token to the header  
in Firefox's network tab:
- Refresh
- Edit & resend 
- Add header

> check http://localhost/productpage -> `401`  

check the response header:

repeat with valid token:
```
Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IkRIRmJwb0lVcXJZOHQyenBBMnFYZkNtcjVWTzVaRXI0UnpIVV8tZW52dlEiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjQ2ODU5ODk3MDAsImZvbyI6ImJhciIsImlhdCI6MTUzMjM4OTcwMCwiaXNzIjoidGVzdGluZ0BzZWN1cmUuaXN0aW8uaW8iLCJzdWIiOiJ0ZXN0aW5nQHNlY3VyZS5pc3Rpby5pbyJ9.CfNnxWP2tcnR9q0vxyxweaF3ovQYHYZl82hAUsn21bwQd9zP7c-LS9qd_vpdLG4Tn1A15NxfCjp5f7QNBUo-KC9PJqYpgGbaXhaGx7bEdFWjcwv3nZzvc7M__ZpaCERdwU7igUmJqYGBYQ51vr2njU9ZimyKkfDe3axcyiBZde7G6dabliUosJvvKOPcKIWPccCgefSj_GNfwIip3-SsFdlR7BtbVUcqR-yv-XOxJ3Uc1MI0tz3uMiiZcyPV7sNCU4KRnemRIMHVOfuvHsU60_GhGbiSFzgPTAa9WTltbnarTbxudb_YEOx12JiwYToeX0DCPb43W1tzIBxgm8NxUg
```
> check http://localhost/productpage -> `200`  
---

### <font color='red'> 3.3.3 Restrict access to productpage </font>
apply a deny-all authorization policy for the product page:
```
kubectl apply -f 03_productpage-authz-deny-all.yaml
```
> check http://localhost/productpage -> `403`  
you need to add an authentication header. In Firefox's network tab:
- Refresh
- Edit & resend 
- Add header

```
Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IkRIRmJwb0lVcXJZOHQyenBBMnFYZkNtcjVWTzVaRXI0UnpIVV8tZW52dlEiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjQ2ODU5ODk3MDAsImZvbyI6ImJhciIsImlhdCI6MTUzMjM4OTcwMCwiaXNzIjoidGVzdGluZ0BzZWN1cmUuaXN0aW8uaW8iLCJzdWIiOiJ0ZXN0aW5nQHNlY3VyZS5pc3Rpby5pbyJ9.CfNnxWP2tcnR9q0vxyxweaF3ovQYHYZl82hAUsn21bwQd9zP7c-LS9qd_vpdLG4Tn1A15NxfCjp5f7QNBUo-KC9PJqYpgGbaXhaGx7bEdFWjcwv3nZzvc7M__ZpaCERdwU7igUmJqYGBYQ51vr2njU9ZimyKkfDe3axcyiBZde7G6dabliUosJvvKOPcKIWPccCgefSj_GNfwIip3-SsFdlR7BtbVUcqR-yv-XOxJ3Uc1MI0tz3uMiiZcyPV7sNCU4KRnemRIMHVOfuvHsU60_GhGbiSFzgPTAa9WTltbnarTbxudb_YEOx12JiwYToeX0DCPb43W1tzIBxgm8NxUg
```
> check http://localhost/productpage -> `200`  

---

### <font color='red'> 3.3.4 Allow access by issuer </font>
apply an authorization policy which allows access by issuer:
```
kubectl apply -f 04_productpage-authz-allow-issuer.yaml
```
> Repeat edit & send request -> `200`
---

### <font color='red'> 3.3.4 Allow access by subject </font>
apply an authorization policy which allows access by subject:
```
kubectl apply -f 04_productpage-authz-allow-subject.yaml
```
> Repeat edit & send request -> `403`
---

### <font color='red'> 3..3.4 Allow access by custom claim </font>
apply an authorization policy which allows access by claim:
```
kubectl apply -f 04_productpage-authz-allow-claim.yaml
```
> Repeat edit & send request -> `200`
---