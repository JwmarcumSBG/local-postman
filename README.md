# local-postman

Postman collections and environments for Mimir, Saga, and Dina APIs.
Connected to Postman desktop via Local View (git-backed workspace).

## Repo Structure

```
postman/
  collections/
    Mimir/          ← Mimir REST API requests
    Mimir REST API/ ← Pulled from Postman cloud (legacy)
    Saga/           ← Saga API requests
    Dina REST API/  ← Dina REST API requests
  environments/
    Mimir Prod.environment.yaml
    Mimir Staging.environment.yaml
    Saga Prod.environment.yaml
    Saga Staging.environment.yaml
    New Environment.environment.yaml
```

## How Postman Local View Works

- Postman desktop is connected to this repo via **Local View** (Files tab)
- Changes to `.request.yaml` and `.environment.yaml` files are picked up automatically by Postman
- Collections map 1:1 to folders under `postman/collections/`
- Each request is its own `.request.yaml` file inside the collection folder

## Adding a New Request

Create a new file in the appropriate collection folder:

```yaml
$kind: http-request
url: "https://us.mjoll.no/ingest/api/v1/generatedMediaLocations"
method: GET
headers:
  x-mimir-cognito-id-token: Bearer {{api_key}}
  Accept: application/json
order: 6000   # controls sort order in Postman sidebar
```

- Use `{{api_key}}` for Mimir auth — resolves from the active environment
- Use `{{saga_api_key}}` for Saga requests
- Use full URLs (not `{{base_url}}`) for endpoints not under `/api/v1/` (e.g. `/ingest/`, `/config/`, `/auth/`)
- `order` is an integer — increment by 1000 for spacing, or 1 for adjacent requests

## Adding a New Collection

Create a new folder under `postman/collections/` with a `.resources/definition.yaml`:

```yaml
$kind: collection
```

Then add request files inside it.

## Environments

| Environment    | Key Variables                        | Notes                          |
|----------------|--------------------------------------|--------------------------------|
| Mimir Prod     | `base_url`, `api_key`                | `base_url` = `https://us.mjoll.no/api/v1` |
| Mimir Staging  | `base_url`, `api_key`                | Same base_url, different key   |
| Saga Prod      | `saga_base_url`, `saga_api_key`      |                                |
| Saga Staging   | `saga_base_url`, `saga_api_key`      |                                |

### IMPORTANT — Environment Values

Postman intentionally does **not** sync environment values to the local repo (security).
Variable names are in the yaml files but values are blank.

**To repopulate values after a fresh setup:**
- API keys and URLs are in `/Users/jaymarcum/git/local-station-onboarding-lab/configs/parameters.local.json`
- Keys: `mimirApiKeySinclairProd`, `mimirApiKeySinclairStaging`, `sagaApiUrl`, `sagaApiKey`
- Enter values manually in Postman by clicking the environment in the sidebar

`folder_id`, `item_id`, and provider folder vars change per session — leave blank.

## Key API Notes

### Mimir Auth Header
```
x-mimir-cognito-id-token: Bearer {{api_key}}
```
Service account keys use `Bearer` prefix. UX tokens (Cognito JWTs from browser) do NOT use `Bearer`.

### Mimir Base URLs by API Group
| API Group | Base Path |
|-----------|-----------|
| Core      | `https://us.mjoll.no/api/v1` |
| Config    | `https://us.mjoll.no/config/api/v1` |
| Ingest    | `https://us.mjoll.no/ingest/api/v1` |
| Auth      | `https://us.mjoll.no/auth/api/v1` |
| Prime     | `https://us.mjoll.no/prime/api/v1` |

Full API reference: `/Users/jaymarcum/git/local-mimir-docs/reference/mimir_api_reference.md`

### Saga Auth Header
```
x-api-key: {{saga_api_key}}
```

## Reference Docs
- Mimir API: `/Users/jaymarcum/git/local-mimir-docs/`
- Saga API: `/Users/jaymarcum/git/local-saga-docs/`
- Mimir onboarding scripts: `/Users/jaymarcum/git/local-station-onboarding-lab/`
