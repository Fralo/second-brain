
### Key endpoints to know

- Authorization: `https://wamdev.roche.com/as/authorization.oauth2`
- Token: `https://wamdev.roche.com/as/token.oauth2`
- MuleSoft Exchange login: `http://login.apis.roche.com/`
- Token validation (JWKS): `https://<pf_host>:<port>/ext/<JWKS Endpoint Path>`


## Trial Simulator
- Is a "Confidential Client with a browser frontend"
- The guide does not mention PKCE -> ask the IAM Team if it can be enabled
- The flow
- FE -> SSO -> Backend (exchanges code for token, saves it in DB and issues http-only Session Cookie) -> FE
- To check if someone has really access we can:
	  1. Filter at the IdP level
		- in this way unauthorized users fail to login and never touch our app
		- ask to roche IAM team
	2. Filter at our backend level
		- we use the `memberOf` claim to check if the user has access
		- OGNL expression to filter `memberOf` to just the groups relevant to TS
		- we could potentially use this for RBAC

## Accessing other APIs (ARC, Prefect deployments)

Section 6.1.2 

- We use the user `AT` to call PingFederate with grant_type=jwt-bearer and using the JWT as assertion
- We get back a new token `AT-Downstream`
- We use this token to access the other API, going througt MuleSoft API Gateway
- We can cache the `AT-Downstream` token in order to avoid going trougth the loop again
	- Other things to keep in mind:
	- TS backend must be registered as allowed to use the `jwt-bearer` grant type.
	- The original JWT needs `iss`, `sub`, `aud`, `exp` claims (the guide is explicit about this).
	- The `aud` of the `AT-Downstream` must match what the downstream MuleSoft API expects

## RBAC

- roles has to be internals to TS
- we can map the memberOf groups to specific roles inside TS app, each role will have it's own permissions
- To clarify with IAM team
	- Can your client be configured with PKCE and group-scoped access?
	- What's the exact claim name for groups, and can it be pre-filtered for your app?
	- Is `jwt-bearer` assertion grant approved TS, and what's the `aud` value for the downstream API?
	- What's the token TTL (`expires_in`) and refresh token TTL? This drives your caching and session strategy.
## Questions
- Will our client have `scope=opendid` that we can use for the OIDC flow?
	- - Can we request `scope=openid` and receive an `id_token`?
	- Is there a discovery endpoint (`/.well-known/openid-configuration`) we should use?
		- https://wamdev.roche.com/.well-known/openid-configuration
			we get 
			```json
			{
  "issuer": "https://wamdev.roche.com",
  "authorization_endpoint": "https://wamdev.roche.com/as/authorization.oauth2",
  "token_endpoint": "https://wamdev.roche.com/as/token.oauth2",
  "revocation_endpoint": "https://wamdev.roche.com/as/revoke_token.oauth2",
  "userinfo_endpoint": "https://wamdev.roche.com/idp/userinfo.openid",
  "introspection_endpoint": "https://wamdev.roche.com/as/introspect.oauth2",
  "jwks_uri": "https://wamdev.roche.com/pf/JWKS",
  "registration_endpoint": "https://wamdev.roche.com/as/clients.oauth2",
  "ping_revoked_sris_endpoint": "https://wamdev.roche.com/pf-ws/rest/sessionMgmt/revokedSris",
  "ping_end_session_endpoint": "https://wamdev.roche.com/idp/startSLO.ping",
  "end_session_endpoint": "https://wamdev.roche.com/idp/startSLO.ping",
  "device_authorization_endpoint": "https://wamdev.roche.com/as/device_authz.oauth2",
  "scopes_supported": [ "address", "edit", "phone", "openid", "domain", "offline_access", "profile", "admin", "email", "register" ],
  "claims_supported": [ "acr", "DN", "Email", "EmployeeType", "FirstName", "LastName", "pi.sri", "sid", "sub" ],
  "response_types_supported": [ "code", "token", "id_token", "code token", "code id_token", "token id_token", "code token id_token" ],
  "response_modes_supported": [ "fragment", "fragment.jwt", "query", "query.jwt", "form_post", "form_post.jwt", "jwt", "pi.flow" ],
  "grant_types_supported": [ "implicit", "authorization_code", "refresh_token", "password", "client_credentials", "urn:pingidentity.com:oauth2:grant_type:validate_bearer", "urn:ietf:params:oauth:grant-type:jwt-bearer", "urn:ietf:params:oauth:grant-type:saml2-bearer", "urn:ietf:params:oauth:grant-type:device_code", "urn:ietf:params:oauth:grant-type:token-exchange", "urn:openid:params:grant-type:ciba" ],
  "subject_types_supported": [ "public", "pairwise" ],
  "id_token_signing_alg_values_supported": [ "none", "HS256", "HS384", "HS512", "RS256", "RS384", "RS512", "ES256", "ES384", "ES512", "PS256", "PS384", "PS512" ],
  "token_endpoint_auth_methods_supported": [ "client_secret_basic", "client_secret_post", "client_secret_jwt", "private_key_jwt", "tls_client_auth", "none" ],
  "token_endpoint_auth_signing_alg_values_supported":  [ "RS256", "RS384", "RS512", "ES256", "ES384", "ES512", "PS256", "PS384", "PS512", "HS256", "HS384", "HS512" ],
  "claim_types_supported": [ "normal" ],
  "claims_parameter_supported": false,
  "request_parameter_supported": true,
  "request_uri_parameter_supported": false,
  "request_object_signing_alg_values_supported": [ "RS256", "RS384", "RS512", "ES256", "ES384", "ES512", "PS256", "PS384", "PS512" ],
  "id_token_encryption_alg_values_supported": [ "dir", "A128KW", "A192KW", "A256KW", "A128GCMKW", "A192GCMKW", "A256GCMKW", "ECDH-ES", "ECDH-ES+A128KW", "ECDH-ES+A192KW", "ECDH-ES+A256KW", "RSA-OAEP", "RSA-OAEP-256" ],
  "id_token_encryption_enc_values_supported": [ "A128CBC-HS256", "A192CBC-HS384", "A256CBC-HS512", "A128GCM", "A192GCM", "A256GCM" ],
  "backchannel_authentication_endpoint": "https://wamdev.roche.com/as/bc-auth.ciba",
  "backchannel_token_delivery_modes_supported": [ "poll", "ping" ],
  "backchannel_authentication_request_signing_alg_values_supported": [ "RS256", "RS384", "RS512", "ES256", "ES384", "ES512", "PS256", "PS384", "PS512" ],
  "backchannel_user_code_parameter_supported": false
}
			```


	- Is there a `userinfo` endpoint and an `end_session_endpoint` (for logout/SLO)?
	- What claims will the `id_token` contain (sub, email, name, groups)?

### What we need

We have to acess this scopes:
- `openid` -> at the platform level, they have that, our client needs to be allowed to use it
- `offline_access` -> this give us the refresh token
```python
scopes: list[str] = ["openid", "profile", "email", "offline_access"]
```

Response type supported:
- we need `code`

Grant type: 
- we should use `authorization_code`
- `subject_types_supported` to check if we have to share it with ARC
	- if it is `public` we can use it to reference the user in other client systems