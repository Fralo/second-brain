
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