# Apache mod_auth_openidc Reverse Proxy with Microsoft Entra ID

This project demonstrates a centralized authentication architecture using:

- Apache HTTP Server
- mod_auth_openidc
- Microsoft Entra ID (Azure AD)
- Azure Web App
- Reverse Proxy Architecture

The objective of this project is to move authentication from application code into the infrastructure layer using Apache Reverse Proxy and OpenID Connect (OIDC).

---

# Project Architecture

```text
User Browser
      │
      ▼
Apache Reverse Proxy VM
(mod_auth_openidc)
      │
      ▼
Microsoft Entra ID
(Authentication)
      │
      ▼
Azure Web App
(Backend Application)
```

---

# Authentication Flow

1. User accesses the Apache Reverse Proxy URL
2. Apache checks whether the user is authenticated
3. If not authenticated, Apache redirects the user to Microsoft Entra ID
4. User logs in using Microsoft credentials
5. Microsoft returns an authorization code to Apache
6. Apache exchanges the authorization code for tokens
7. Apache validates the token and creates a secure session
8. Apache forwards the authenticated request to the backend Azure Web App

---

# Technologies Used

| Component | Purpose |
|---|---|
| Apache HTTP Server | Reverse Proxy |
| mod_auth_openidc | OpenID Connect Authentication |
| Microsoft Entra ID | Identity Provider |
| Azure VM | Apache Hosting |
| Azure Web App | Backend Application |
| OpenID Connect (OIDC) | Authentication Protocol |

---

# Key Features

- Centralized Authentication
- Reverse Proxy Security
- Microsoft SSO Integration
- Token Validation
- Secure HTTPS Communication
- Infrastructure-Level Authentication
- Simplified Backend Applications

---

# Apache Configuration File

Main Apache configuration file:

```bash
/etc/apache2/sites-available/default-ssl.conf
```

---

# Example Apache Reverse Proxy Configuration

```apache
<VirtualHost *:443>

SSLEngine on

OIDCProviderMetadataURL https://login.microsoftonline.com/TENANT_ID/v2.0/.well-known/openid-configuration

OIDCClientID YOUR_CLIENT_ID

OIDCClientSecret YOUR_CLIENT_SECRET

OIDCRedirectURI https://YOUR_VM_IP/redirect_uri

OIDCCryptoPassphrase randomSecret123

OIDCRemoteUserClaim sub

OIDCScope "openid profile email"

SSLProxyEngine On

ProxyPass / https://YOUR_WEBAPP.azurewebsites.net/

ProxyPassReverse / https://YOUR_WEBAPP.azurewebsites.net/

<Location />
    AuthType openid-connect
    Require valid-user
</Location>

</VirtualHost>
```

---

# Ports Used

| Port | Purpose |
|---|---|
| 80 | HTTP Redirect |
| 443 | HTTPS Secure Access |

---

# Security Benefits

- Authentication removed from application code
- Centralized session management
- Secure token validation
- HTTPS encrypted communication
- Easier enterprise scalability
- Single Sign-On (SSO)

---

# Practical Use Cases

This architecture is commonly used in:

- Enterprise SSO Platforms
- API Gateways
- Kubernetes Ingress Authentication
- Zero Trust Architectures
- Internal Corporate Applications

---

# Future Improvements

- Multi-Application Reverse Proxy
- Azure AD Group Authorization
- MFA Integration
- Conditional Access Policies
- WAF Integration
- Kubernetes Deployment

---

# Author

Your Name

---

# License

This project is for educational and learning purposes.
