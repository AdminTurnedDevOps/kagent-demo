Update all of the configs below:

```
export BACKEND_CLIENT_ID=kagent-backend
export BACKEND_CLIENT_SECRET=EbNAi5Rqb9aE9ycicUgdqXDhx1mdzdCu
export BACKEND_ISSUER=https://35.237.149.144/realms/master
export FRONTEND_CLIENT_ID=kagent-frontend
export FRONTEND_AUTH_ENDPOINT=https://35.237.149.144/realms/master/protocol/openid-connect/auth
export FRONTEND_LOGOUT_ENDPOINT=https://35.237.149.144/realms/master/protocol/openid-connect/logout
export FRONTEND_TOKEN_ENDPOINT=https://35.237.149.144/realms/master/protocol/openid-connect/token
export CLUSTER1=gke_field-engineering-us_us-east1_mgmtclus01
```
### Management Cluster
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

### Worker Cluster
```
export TUNNEL_ADDRESS=$(kubectl get svc -n kagent kagent-enterprise-ui --context=$CLUSTER1 -o jsonpath="{.status.loadBalancer.ingress[0]['hostname','ip']}")
echo $TUNNEL_ADDRESS
```

```
export CLUSTER2=gke_field-engineering-us_us-central1_workerclus01
export CLUSTER2_NAME=workerclus01

kubectl apply --context=$CLUSTER1 -f- <<EOF
apiVersion: kagent-enterprise.solo.io/v1alpha1
kind: KubernetesCluster
metadata:
  name: $CLUSTER2_NAME
  namespace: kagent
EOF
```

```
helm upgrade -i relay \
oci://us-docker.pkg.dev/solo-public/kagent-enterprise-helm/charts/relay \
--kube-context $CLUSTER2 \
-n kagent --create-namespace \
--version 0.1.0 \
--set cluster=$CLUSTER2_NAME \
--set tunnel.endpoint=${TUNNEL_ADDRESS} \
--set telemetry.endpoint=${TUNNEL_ADDRESS}
```

```
kubectl get pods -n kagent --context=$CLUSTER2
```