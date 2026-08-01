---
name: Provision and validate a Quadrillion API key
description: Create a Quadrillion Cloud API key, confirm it is valid, and use it to proxy an LLM request.
api: openapi/quadrillion-cloud-openapi-original.json
operations: [create_api_key_api_keys_post, validate_api_key_api_keys_validate_post, get_available_models_models_get, proxy_response_anthropic_v1_messages_post]
---

# Provision and validate a Quadrillion API key

Programmatic access to the Quadrillion Cloud API (`https://api.quadrillion.io`) is authorized with an API key. Follow these steps.

## Steps

1. **Create a key** — `POST /api/keys` (`create_api_key_api_keys_post`). The response returns the plaintext `key` exactly once with the warning "Save this key now. You won't be able to see it again!" Persist it in a secret store immediately.
2. **Validate the key** — `POST /api/keys/validate` (`validate_api_key_api_keys_validate_post`) to confirm the key is active before wiring it into an integration.
3. **Discover models** — `GET /models` (`get_available_models_models_get`) to see which models the account can call.
4. **Proxy an LLM request** — send the key as the auth header and call a provider proxy route, e.g. `POST /v1/messages` (`proxy_response_anthropic_v1_messages_post`) for Anthropic-shaped requests, or `POST /responses` for OpenAI-shaped requests.

## Rules

- The plaintext key is shown only at creation; there is no retrieval endpoint — regenerate (`DELETE /api/keys/{key_id}` then recreate) if lost.
- Errors are FastAPI-style: request-validation failures return HTTP 422 with a `detail[]` array (`loc`/`msg`/`type`). See `errors/quadrillion-problem-types.yml`.
- The API declares no idempotency contract — do not assume safe retries on non-idempotent POSTs (`conventions/quadrillion-conventions.yml`).
