1. `auth/keycloak-demo.yaml`

2. `kagent/installKagent.md`

3. `kagent/kagent-gateway.yaml`

4. `auth/keycloakSetup.md` WITH the IP address from the Gateway created in the previous step.

5. Get the IP of the Gateway
```
kubectl get gateway -n kagent
```
65. Re-run `kagent/installKagent.md` with the public Gateway IP under `kubectl get gateway -n kagent`.

The reason why you have to re-run is because the `keycloakSetup.md` requires the Kagent Enterprise UIs Gateway IP address for the redirect logins. In production, you'd have an OIDC provider configured already, so this step wouldn't be needed. It's just needed for Keycloak.

Keycloak still runs on its own LoadBalancer service The Gateway doesn't replace that LoadBalancer - it just adds an additional route to access Keycloak.

Here's how it works:

  Two ways to access Keycloak now:
  1. Direct LoadBalancer: http://keycloak_lb:8080 (still works)
  2. Through Gateway: https://gateway_ip/realms (new proxy route)

Why we need both:
  - The LoadBalancer service is still there for admin access and direct API calls
  - The Gateway proxy is specifically for browser-based flows from the kagent UI to avoid mixed
  content errors

Traffic flow:
  User browser → https://gateway_ip/ → kagent UI
  User browser → https://gateway_ip/realms → Keycloak (proxied)

This way, both the UI and Keycloak appear to come from the same origin (35.237.149.144) to the browser, solving the mixed content security issue while keeping Keycloak's original LoadBalancer intact for other uses.