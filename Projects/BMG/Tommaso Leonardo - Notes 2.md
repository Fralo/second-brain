---
type: meeting-notes
date: 2026-05-12
created: 2026-05-12 10:30
project: BMG
attendees:
  - Tommaso Meledina (Tech Lead)
---
## Attendees
- Tommaso Meledina (Tech Lead)

---

## Context

The next BMG quarter (QVR 2) will focus mostly on **non-functional requirements** rather than new features:
- Authentication
- Environments
- Monitoring / observability

> QVR = "quadrimestre" — BMG's four-month delivery cycle.

---

## Authentication

### Planned for QVR 2

- Roche has an **internal authentication system** (they call it SSO) which is expected to comply with the **OIDC** protocol.
- If it does comply with OIDC, the integration on our side is straightforward: the frontend app redirects to Roche's identity provider, and the user comes back with a token.
- That token contains **claims** identifying the user. From there we want to adopt an **On-Behalf-Of (OBO)** pattern for service-to-service calls.

### Service-to-service calls — two options

When our app needs to call other services to enrich a response, we have two patterns to choose from:

#### Option A — On-Behalf-Of (OBO)
- We attach an authorization token to the downstream call, propagating **who the end user is**.
- The downstream service sees the original user identity and can apply user-level authorization.

#### Option B — Client Credentials (CC)
- Our app talks to the OIDC provider as itself (not on behalf of the user) to obtain a service token.
- The provider tells the downstream service:
	- who the request is coming from (which app/client)
	- what the request is for (scope)
- The downstream service uses the **scope** to decide whether the caller is authorized.
- More complex to set up, especially for whoever manages the auth provider.
- ❓ *Tommaso also mentioned "condensing the token" in this flow — meaning unclear, to be clarified next time.*

---

## Environments

- Today Roche only provides a **development environment**.
- During QVR 2 we need to get **test** and **production** environments set up as well.

---

## Monitoring / Observability

- Our applications already emit **traces and metrics** following the **OpenTelemetry** protocol.
- Tommaso has simulated a local setup with OpenTelemetry + Grafana to validate this.
- Next step: align with **the client's expectations** for observability.
	- The client doesn't yet have clear ideas on this.
	- They don't have a standard way of doing observability internally.

---

## Keywords / follow-ups

- OIDC, OAuth2
- OBO vs Client Credentials — pick a default pattern
- Clarify what "condensing the token" referred to
- Define test + prod environments with Roche
- Agree on an observability standard with the client
