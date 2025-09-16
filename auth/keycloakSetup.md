Access Keycloak at: http://YOUR_LB_IP:8080
- Admin Console: http://YOUR_LB_IP:8080/admin

Login:
Username: admin
Password: admin123

Here are the step-by-step instructions for creating OIDC clients inKeycloak:

  Backend Client (Confidential)

  1. Navigate to Clients:
    - In Keycloak Admin Console, click "Clients" in left sidebar
  2. Create New Client:
    - Click "Create client" button
    - Client type: OpenID Connect
    - Client ID: kagent-backend
    - Click "Next"
  3. Capability Config:
    - Client authentication: ON (toggle to enabled)
    - Authorization: OFF
    - Standard flow: ON
    - Service accounts roles: ON
    - Click "Next", then "Save"
  4. Get Client Secret:
    - Go to "Credentials" tab
    - Copy the "Client secret" value

  Frontend Client (Public)

  1. Create New Client:
    - Click "Create client" again
    - Client type: OpenID Connect
    - Client ID: kagent-frontend
    - Click "Next"
  2. Capability Config:
    - Client authentication: OFF (toggle to disabled)
    - Standard flow: ON
    - Direct access grants: ON
    - Click "Next"
  3. Login Settings:
    - Valid redirect URIs: * (for demo purposes)
    - Web origins: *
    - Click "Save"