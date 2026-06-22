```
# PostgreSQL service configuration
DATABASE_NAME=testdb
DATASOURCE_USER=dbuser
DATASOURCE_PASSWORD=changeme

# Application settings (DATABASE_URL is built in docker-compose from vars below)
APP_NAME=trial-simulator-be
LOG_LEVEL=DEBUG
LOG_MODE=json
OTEL_EXPORT_ENABLED=true
OTEL_EXPORTER_ENDPOINT=http://otel-collector:4317

# FE observability (browser-side telemetry — off by default, collector rarely running)
FE_OTEL_EXPORT_ENABLED=false
FE_OTEL_EXPORTER_ENDPOINT=http://localhost:4318

# ── Auth: Roche SSO (Ping) QA tenant ─────────────────────────────────
# Wired to wamqa.roche.com for the `tsptest` confidential client. The
# BFF resolves /.well-known/openid-configuration on startup; the JWKS,
# token, and authorize URIs below are pinned for clarity but the BFF
# will use the discovery values when these are empty.
AUTH_TYPE=ping
PROFILE=mock
AUTH_ENABLED=true
BYPASS_PRINCIPAL_ROLES=
ALLOWED_HOSTS=localhost,127.0.0.1,trial-simulator-be
TRUST_PROXY_HOPS=1

AUTH_EXTERNAL_ISSUER=https://wamqa.roche.com
AUTH_EXTERNAL_DISCOVERY_URI=https://wamqa.roche.com/.well-known/openid-configuration
AUTH_EXTERNAL_JWKS_URI=https://wamqa.roche.com/pf/JWKS
AUTH_EXTERNAL_TOKEN_URI=https://wamqa.roche.com/as/token.oauth2
AUTH_EXTERNAL_AUTHORIZE_URI=https://wamqa.roche.com/as/authorization.oauth2
AUTH_EXTERNAL_CLIENT_ID=tsptest
AUTH_EXTERNAL_CLIENT_SECRET=0oyr7mqnpxjneyhg2egn97dvuv67ije95x1uh78qvhed2pw2n2ybzr99f1heicxe
AUTH_EXTERNAL_REDIRECT_URI=http://localhost:4200/auth/callback
AUTH_EXTERNAL_AUDIENCE=

# Sealing keyring (Fernet) and CSRF signing key. Generated locally for
# this dev exercise; rotate before any non-dev use.
AUTH_SESSION_KEYS=7_T5tzLy4Suq8Ij9m1fqB7-svehl_OqMnstHBcGYVEg=
AUTH_CSRF_KEY=gnluWFJjh9GFHdnMTvIl2_Vcmdbhv2-tvkzDs7BMTpw

# Verbose audit + short check period so the IdP-freshness path exercises
# quickly while we're observing logs.
AUTH_AUDIT_SINK=stdout
AUTH_SESSION_IDLE_SECONDS=1800
AUTH_SESSION_ABSOLUTE_SECONDS=28800
AUTH_COOKIE_CHECK_PERIOD_MINUTES=1

# ARC integration
ARC_API_BASE_URL=http://integrations-mock:8080

# Project version (must match the bundled ARC snapshot)
DEFAULT_PROJECT_UUID=zNxBWOvy4omBvKzPEAPWZ0oXYd8Vo3KQ
DEFAULT_PROJECT_VERSION_ID=boED54WLMRZYdnOmjYVRLYGwjB9BKrxp
ARC_DEFAULT_PROJECT_VERSION_ID=boED54WLMRZYdnOmjYVRLYGwjB9BKrxp

# BE build context (relative to compose/ directory)
BE_CONTEXT_PATH=../trial-simulator-be/
BE_DOCKERFILE_PATH=Dockerfile

# FE build context (relative to compose/ directory)
FE_CONTEXT_PATH=../trial-simulator-fe/
FE_DOCKERFILE_PATH=Dockerfile

# Path to user-level .npmrc file (used as a Docker secret for npm auth)
NPMRC_PATH=~/.npmrc

# OTEL Collector image (contrib includes jaeger and prometheus exporters)
OTELCOL_IMG=otel/opentelemetry-collector-contrib:latest

```
