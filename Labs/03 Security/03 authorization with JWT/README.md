## <font color='orange'> 4.3 End-user Authorization </font>
require end-user authentication with JWT and enforce access control based on the JWT calims.

### <font color='orange'> 4.3.1 Require JWT  </font>
apply the JWT authentication policy for the product page:
```
kubectl apply -f 01_productpage-auth-jwt.yaml
```
> check http://localhost/productpage -> `401`  
> check http://localhost:6324/productpage -> `401`

you need to add an authentication header. In Firefox's network tab:

- Refresh
- Edit & resend 
- Add header

```
Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IkRIRmJwb0lVcXJZOHQyenBBMnFYZkNtcjVWTzVaRXI0UnpIVV8tZW52dlEiLCJ0eXAiOiJKV1QifQ.eyJleHAiOjQ2ODU5ODk3MDAsImZvbyI6ImJhciIsImlhdCI6MTUzMjM4OTcwMCwiaXNzIjoidGVzdGluZ0BzZWN1cmUuaXN0aW8uaW8iLCJzdWIiOiJ0ZXN0aW5nQHNlY3VyZS5pc3Rpby5pbyJ9.CfNnxWP2tcnR9q0vxyxweaF3ovQYHYZl82hAUsn21bwQd9zP7c-LS9qd_vpdLG4Tn1A15NxfCjp5f7QNBUo-KC9PJqYpgGbaXhaGx7bEdFWjcwv3nZzvc7M__ZpaCERdwU7igUmJqYGBYQ51vr2njU9ZimyKkfDe3axcyiBZde7G6dabliUosJvvKOPcKIWPccCgefSj_GNfwIip3-SsFdlR7BtbVUcqR-yv-XOxJ3Uc1MI0tz3uMiiZcyPV7sNCU4KRnemRIMHVOfuvHsU60_GhGbiSFzgPTAa9WTltbnarTbxudb_YEOx12JiwYToeX0DCPb43W1tzIBxgm8NxUg
```

> http://localhost/productpage -> `200`  
> http://localhost:6324/productpage -> `200`

### <font color='orange'> 3.2 Restrict access to productpage </font>
apply a deny-all authorization policy for the product page:
```
kubectl apply -f productpage-authz-deny-all.yaml
```

> Repeat edit & send request -> `403`

### <font color='orange'> 3.3 Decode the JWT </font>
The JWT is a base64 encoded string. Read the claims - browse to 
 https://jwt.io and paste contents of [demo.jwt](demo.jwt)

- Issuer: `testing@secure.istio.io`
- Subject: `testing@secure.istio.io`
- Custom: `foo=bar`

## 3.4 Allow access by issuer

Apply an [authorization policy which allows access by issuer](productpage-authz-allow-issuer.yaml):

```
kubectl apply -f productpage-authz-allow-issuer.yaml
```
> Repeat edit & send request -> `200`

## 3.5 Allow access by subject

Apply an [authorization policy which allows access by subject](productpage-authz-allow-subject.yaml):

```
kubectl apply -f productpage-authz-allow-subject.yaml
```
> Repeat edit & send request -> `403`

## 3.6 Allow access by custom claim

Apply an [authorization policy which allows access by claim](productpage-authz-allow-claim.yaml):

```
kubectl apply -f productpage-authz-allow-claim.yaml
```

> Repeat edit & send request -> `200`