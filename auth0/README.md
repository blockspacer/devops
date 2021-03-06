# Auth0

## openid connect configuration

https://YOUR_DOMAIN/.well-known/openid-configuration

## Implicit Flow

https://auth0.com/docs/flows/guides/implicit/add-login-implicit

https://YOUR_DOMAIN/authorize?
    response_type=YOUR_RESPONSE_TYPE&
    client_id=YOUR_CLIENT_ID&
    redirect_uri=https://YOUR_APP/callback&
    state=STATE&
    nonce=NONCE

## Client Credential

https://auth0.com/docs/api-auth/tutorials/client-credentials

curl --request POST \
  --url 'https://YOUR_DOMAIN/oauth/token' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data grant_type=client_credentials \
  --data 'client_id=YOUR_CLIENT_ID' \
  --data client_secret=YOUR_CLIENT_SECRET \
  --data audience=YOUR_API_IDENTIFIER

{
 "access_token":"eyJz93a...k4laUWw",
 "token_type":"Bearer",
 "expires_in":86400
}

## Test API

curl --request GET \
  --url http://path_to_your_api/ \
  --header 'authorization: Bearer <TOKEN>'

## Istio

````
apiVersion: authentication.istio.io/v1alpha1
kind: Policy
metadata:
  name: ingressgateway
  namespace: istio-system
spec:
  targets:
  - name: istio-ingressgateway
  peers:
  - mtls: {}
  origins:
  - jwt:
      audiences:
      - "YOUR_API_IDENTIFIER"
      issuer: "YOUR_DOMAIN"
      jwksUri: "YOUR_DOMAIN/.well-known/jwks.json"
  principalBinding: USE_ORIGIN`
````