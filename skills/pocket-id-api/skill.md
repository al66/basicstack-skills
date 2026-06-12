# Pocket ID API

Use this skill when users need to work with the Pocket ID API.

## Required secret

- `POCKET_ID_API_KEY` (provided via environment variable)

## Instructions

1. Read the API description shared by the user and follow its endpoint, method, and payload definitions exactly.
2. Authenticate all API requests with the injected key from `POCKET_ID_API_KEY`.
3. Never hardcode API keys, tokens, or credentials in code, examples, or logs.
4. Prefer concise, production-safe request examples (for example with `curl`) that use environment variables.

## Example authentication pattern

```bash
curl -H "X-API-Key: $POCKET_ID_API_KEY" "<POCKET_ID_API_ENDPOINT>"
```
