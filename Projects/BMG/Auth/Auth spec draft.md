
Authentication and authorization design for the Product subsystem (Angular SSR frontend + FastAPI backend, integrating with Roche - PingFederate OIDC provider)

# 01 — Design Principles

This file lists the _a priori_ commitments that drive every design choice in the rest of this specification. Every other file should be traceable back to one or more principles listed here.

If a principle and a downstream design choice conflict, the principle wins — or the principle is wrong and should be revised explicitly (via an ADR), not silently violated.

---

## P1 — Confidential client pattern

The OIDC `client_secret` lives only on the backend, in environment variables loaded at startup. It never appears in browser-side code, client-side configuration, frontend bundles, or any artifact the user agent can access.

**Why:** the secret is the basis of the trust relationship between our application and the OpenID Provider. A leaked secret means anyone can impersonate our application to the IdP.

**Where this lands:** [`config.py`](https://claude.ai/snippets/config.py), [`oidc_client.py`](https://claude.ai/snippets/oidc_client.py).

---

## P2 — Backend for Frontend (BFF)

The browser never holds OAuth or OIDC tokens. The frontend interacts with the backend through a session cookie; the backend holds and manages the tokens server-side.

**Why:** tokens in browser storage (localStorage, sessionStorage, IndexedDB) are reachable by XSS. Tokens in JavaScript memory are reachable by any extension or injected script. A session cookie marked HttpOnly is reachable by neither.

**Trade-off accepted:** the frontend becomes coupled to the backend for auth state, losing some independence. We consider this acceptable for a single-product subsystem; the alternative (SPA-with-tokens) would shift risk in ways we are not willing to accept.

**Where this lands:** [`router.py`](https://claude.ai/snippets/router.py), [`session.py`](https://claude.ai/snippets/session.py), [ADR 0001](https://claude.ai/decisions/0001-bff-over-spa-tokens.md).

---

## P3 — Server-side session, opaque cookie

User sessions are stored server-side (Redis). The browser receives only an opaque random identifier in an HTTP-only, Secure, SameSite=Lax cookie. The identifier carries no information; it is a key into the session store.

**Why:** server-side sessions can be revoked instantly (delete the key), allow per-user state that doesn't fit in a cookie (tokens, groups, permissions), and don't grow with claims. They are also straightforward to inspect and audit.

**Trade-off accepted:** the backend needs a session store as a hard dependency. Redis is fast enough that this is not a performance concern.

**Where this lands:** [`session.py`](https://claude.ai/snippets/session.py), [ADR 0003](https://claude.ai/decisions/0003-redis-session-store.md).

---

## P4 — Defense in depth on authorization

Coarse-grained access control (who can use the app at all) is enforced in at least two places:

1. **Optionally at the IdP**, by binding our client registration to one or more AD groups, so users outside those groups cannot complete login at all.
2. **Always at the backend**, by re-checking group claims in the id_token and rejecting login if the user is not in an allowed group.

The backend check is mandatory regardless of whether the IdP also checks. We do not assume that upstream controls remain in place forever.

**Why:** group membership at the IdP is configurable by people outside our team. We must be able to enforce our own access policy independently.

**Where this lands:** [`router.py`](https://claude.ai/snippets/router.py) (the `callback` endpoint), [`config.py`](https://claude.ai/snippets/config.py) (the `allowed_groups` setting).

---

## P5 — Separation of authentication and authorization concerns

|Concern|Owner|
|---|---|
|Who is the user?|OpenID Provider (PingFederate)|
|Can the user access this app at all?|Group claims in the id_token|
|What can the user do inside the app?|Application database (roles, permissions)|

The IdP authenticates and supplies identity claims. Coarse-grained access is decided from claims in the token. Fine-grained access (RBAC) is decided from data the application owns and can evolve independently.

**Why:** RBAC changes frequently as features evolve. AD groups change slowly and require IT process. Coupling fine-grained permissions to AD groups would either ossify our product or generate sprawl in IAM's group catalog. The indirection lets each layer evolve at its natural pace.

**Where this lands:** [`resolver.py`](https://claude.ai/snippets/resolver.py), [`06-authz-model.md`](https://claude.ai/chat/06-authz-model.md).

---

## P6 — Least privilege on downstream tokens

When calling a downstream API, we never forward the user's primary access token. We exchange it for a new token scoped to the downstream audience, using the JWT Bearer Assertion grant (RFC 7523).

**Why:** the original access token has whatever `aud` the IdP set for our client. The downstream API will reject it (correct behavior) or worse, accept it for an unintended audience. Exchanging the token also gives the IdP an audit point for cascaded access.

**Trade-off accepted:** one extra round trip to the IdP per downstream API per session. Mitigated by caching exchanged tokens per `(user, audience)` until shortly before expiry.

**Where this lands:** [`token_exchange.py`](https://claude.ai/snippets/token_exchange.py), [`05-token-exchange.md`](https://claude.ai/chat/05-token-exchange.md).

---

## P7 — Pin the signing algorithm

JWT validation explicitly pins the expected signing algorithm (`RS256` by default). We never accept `alg: none`, and we never let the token's own header dictate the verification algorithm.

**Why:** the "algorithm confusion" attack family (including `alg: none` and HS256/RS256 substitution) requires that the verifier trust the token's self-declared algorithm. Pinning removes the attack surface entirely.

**Where this lands:** [`oidc_client.py`](https://claude.ai/snippets/oidc_client.py) (the `id_token_signed_response_alg` setting).

---

## P8 — Discovery over hardcoding

All OIDC endpoints (authorization, token, JWKS, userinfo, end_session) are read from the OpenID Provider Metadata document at `${ISSUER}/.well-known/openid-configuration`. Endpoint URLs are never hardcoded in source.

**Why:** environment promotion (dev → test → stage → prod) requires changing only the issuer URL. Endpoint paths and supported algorithms are properties of the IdP and may change over its lifetime; we should not encode them as constants in our codebase.

**Where this lands:** [`oidc_client.py`](https://claude.ai/snippets/oidc_client.py), [`10-environments-and-config.md`](https://claude.ai/chat/10-environments-and-config.md).

---

## P9 — PKCE on every authorization request

Every authorization request uses PKCE with `code_challenge_method=S256`, regardless of whether the client is confidential.

**Why:** PKCE protects against authorization code interception by binding the code to a verifier known only to the original requester. Although originally introduced for public clients, it is now standard practice for confidential clients too (OAuth 2.1 mandates it). The cost is negligible.

**Where this lands:** [`oidc_client.py`](https://claude.ai/snippets/oidc_client.py).

---

## P10 — Stateless JWTs, stateful sessions

JWTs (id_token, access_token) are validated per the OIDC spec on every use — signature, issuer, audience, expiry, nonce. We do not maintain state about JWTs locally.

User sessions, by contrast, are stateful: a session ID maps to a Redis record that we can read, update, and delete. Session lifecycle is under our control, not the IdP's.

**Why:** JWTs are designed to be self-validating; treating them as stateless avoids cache coherence issues with the IdP. Sessions are our contract with the user; treating them as stateful gives us revocation, sliding TTL, and per-user metadata.

**Where this lands:** [`session.py`](https://claude.ai/snippets/session.py), [`dependencies.py`](https://claude.ai/snippets/dependencies.py).

---

## P11 — Fail closed

When any check is ambiguous or any dependency is unreachable, the default is to deny access, not grant it. Specific examples:

- Session lookup fails (Redis down) → 401, do not proceed without a session
- JWKS fetch fails during token validation → reject the token
- Group claim is missing from id_token → reject login (do not default to "no groups, no access" silently — log and surface as an explicit error)
- Permission resolution returns an empty set when the user is known to have groups → treat as a configuration error, deny

**Why:** the cost of a denied legitimate request is a retry. The cost of a granted unauthorized request can be unbounded.

**Where this lands:** [`dependencies.py`](https://claude.ai/snippets/dependencies.py), [`09-error-handling.md`](https://claude.ai/chat/09-error-handling.md).

---

## P12 — Observability is part of the design, not bolted on

Every authorization decision (allow, deny, login success, login failure, token exchange, refresh) emits a structured log entry with at minimum: timestamp, user `sub` (when known), decision, reason. PII (email, name) is excluded from log entries unless explicitly required for a specific audit case.

**Why:** auth issues are notoriously hard to debug after the fact. Without structured logs we are blind. PII exclusion limits the blast radius of a log leak.

**Where this lands:** to be specified in [`09-error-handling.md`](https://claude.ai/chat/09-error-handling.md) and applied throughout the snippets.

---

## How to use these principles

When reviewing a PR or considering a design change, run through this list. If the change conflicts with a principle, one of three things is true:

1. The change is wrong — adjust the change.
2. The principle is wrong — propose an ADR superseding it.
3. The principle does not apply — say so explicitly in the PR description so the next reviewer understands.

Silent violations are the failure mode this document exists to prevent.