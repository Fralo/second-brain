
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