Access Keycloak at: http://YOUR_LB_IP:8080
- Admin Console: http://YOUR_LB_IP:8080/admin

Login:
Username: admin
Password: admin123

Here are the step-by-step instructions for creating OIDC clients inKeycloak:
  1. Access Keycloak Admin Console

  - Go to: http://34.26.60.83:8080/admin
  - Login: admin / admin123

  2. Create kagent-frontend Client

  1. Click Clients in left menu
  2. Click Create client button
  3. Fill in:
    - Client type: OpenID Connect
    - Client ID: kagent-frontend
    - Click Next
  4. On next screen:
    - Client authentication: Off
    - Authorization: Off
    - Standard flow: On ✓
    - Direct access grants: On ✓
    - Click Next
  5. On Login settings:
    - Valid redirect URIs: https://35.237.149.144/*
    - Valid post logout redirect URIs: https://35.237.149.144/*
    - Web origins: https://35.237.149.144
    - Click Save

  3. Create kagent-backend Client

  1. Click Create client button again
  2. Fill in:
    - Client type: OpenID Connect
    - Client ID: kagent-backend
    - Click Next
  3. On next screen:
    - Client authentication: On ✓
    - Authorization: Off
    - Standard flow: On ✓
    - Service accounts roles: On ✓
    - Click Next
  4. On Login settings:
    - Valid redirect URIs: https://35.237.149.144/*
    - Click Save

  4. Get Client Secret

  1. Click on kagent-backend client
  2. Go to Credentials tab
  3. Copy the Client secret value - you'll need this for kagent configuration