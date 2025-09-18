# Kagent Enterprise Installation

## 1. Deploy Keycloak
```bash
kubectl apply -f keycloak.yaml
kubectl get svc keycloak
```

Wait for LoadBalancer IP and access Keycloak at: http://KEYCLOAK_IP:8080
- Admin Console: http://KEYCLOAK_IP:8080/admin
- Login: admin / password

## 2. Configure OIDC Clients in Keycloak

### Create kagent-frontend Client
1. Go to Clients → Create client
2. Client ID: `kagent-frontend`
3. Client authentication: OFF
4. Standard flow: ON
5. Valid redirect URIs: `http://KAGENT_UI_IP/*`
6. Web origins: `http://KAGENT_UI_IP`

### Create kagent-backend Client  
1. Go to Clients → Create client
2. Client ID: `kagent-backend`
3. Client authentication: ON
4. Standard flow: ON
5. Service accounts roles: ON
6. Get client secret from Credentials tab

## 3. Install Kagent Enterprise

```bash
# export CLUSTER1=your-cluster-context
# export KEYCLOAK_IP=your-keycloak-ip
# export BACKEND_CLIENT_SECRET=your-backend-secret
export BACKEND_CLIENT_SECRET=IntPVCsnCifNrvFuzScsVWGG5vxK5Vih
export CLUSTER1=gke_field-engineering-us_us-east1_mgmtclus01
export KEYCLOAK_IP=34.23.181.61


helm upgrade -i kagent-enterprise \
oci://us-docker.pkg.dev/solo-public/kagent-enterprise-helm/charts/management \
-n kagent --create-namespace \
--version 0.1.0 \
-f - <<EOF
cluster: $CLUSTER1
ui:
  backend:
    oidcClientId: kagent-backend
    oidcSecret: ${BACKEND_CLIENT_SECRET}
    oidcIssuer: http://${KEYCLOAK_IP}:8080/realms/master
  frontend:
    clientId: kagent-frontend
    authEndpoint: http://${KEYCLOAK_IP}:8080/realms/master/protocol/openid-connect/auth
    logoutEndpoint: http://${KEYCLOAK_IP}:8080/realms/master/protocol/openid-connect/logout
    tokenEndpoint: http://${KEYCLOAK_IP}:8080/realms/master/protocol/openid-connect/token
EOF
```

## 4. Access UI
```bash
kubectl get svc kagent-enterprise-ui -n kagent
```
Access at: http://KAGENT_UI_IP