Update all of the configs below:

```
export BACKEND_CLIENT_ID=
export BACKEND_CLIENT_SECRET=
export BACKEND_ISSUER=http://KEYCLOAK_LB_IP:8080/realms/master
export FRONTEND_CLIENT_ID=
export FRONTEND_AUTH_ENDPOINT=http://KEYCLOAK_LB_IP:8080/realms/master/protocol/openid-connect/auth
export FRONTEND_LOGOUT_ENDPOINT=http:/KEYCLOAK_LB_IP:8080/realms/master/protocol/openid-connect/logout
export FRONTEND_TOKEN_ENDPOINT=http://KEYCLOAK_LB_IP:8080/realms/master/protocol/openid-connect/token
export CLUSTER1=
```

```
helm upgrade -i kagent-enterprise --kube-context=$CLUSTER1 \
oci://us-docker.pkg.dev/solo-public/kagent-enterprise-helm/charts/management \
-n kagent --create-namespace \
--version 0.1.0 \
-f - <<EOF
cluster: $CLUSTER1
ui:
  backend:
    oidcClientId: ${BACKEND_CLIENT_ID}
    oidcSecret: ${BACKEND_CLIENT_SECRET}
    oidcIssuer: ${BACKEND_ISSUER}
  frontend:
    clientId: ${FRONTEND_CLIENT_ID}
    authEndpoint: ${FRONTEND_AUTH_ENDPOINT}
    logoutEndpoint: ${FRONTEND_LOGOUT_ENDPOINT}
    tokenEndpoint: ${FRONTEND_TOKEN_ENDPOINT}
EOF
```

```
kubectl get pods -n kagent
```