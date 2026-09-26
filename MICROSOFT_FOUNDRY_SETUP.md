# Microsoft Foundry Setup for PolicyTrace

This file documents the Microsoft Foundry configuration used for the live PolicyTrace contest validation.

## Validated configuration

- Provider: **Microsoft Foundry**
- Deployment / model: `gpt-5-mini`
- Deployment type shown in Foundry: **Global Standard**
- Models resource endpoint used by PolicyTrace:
  `https://policytrance.services.ai.azure.com`
- Authentication used during live validation: **API key**

> The endpoint spelling above is intentional: `policytrance`.

## Important endpoint distinction

PolicyTrace currently uses the Microsoft Foundry / Azure OpenAI-compatible **Chat Completions** route.

The application expects the **Models resource endpoint root**, for example:

```text
https://policytrance.services.ai.azure.com
```

From that root, the client constructs:

```text
https://policytrance.services.ai.azure.com/openai/v1/chat/completions
```

Do **not** enter a project-level Responses endpoint such as:

```text
https://policytrance.services.ai.azure.com/api/projects/proj-default/openai/v1/responses
```

The current client intentionally rejects `/api/projects/` endpoints because that is not the route used by this build.

## Option 1: configure in the PolicyTrace UI

1. Start the application:

   ```bash
   python -m pip install -r backend/requirements.txt
   python backend/run_app.py
   ```

2. Open:

   ```text
   http://127.0.0.1:8777/
   ```

3. In the provider settings, choose **Microsoft Foundry**.

4. Enter:

   - Endpoint: `https://policytrance.services.ai.azure.com`
   - Model / deployment: `gpt-5-mini`
   - API key: your Microsoft Foundry resource key
   - Bearer token: leave blank when using an API key

5. Run the policy intake and analysis normally.

Provider credentials are kept in process memory and are not written to the saved PolicyTrace project JSON.

## Option 2: configure with environment variables

PolicyTrace also reads the following environment variables:

```text
POLICYTRACE_FOUNDRY_ENDPOINT
POLICYTRACE_FOUNDRY_MODEL
POLICYTRACE_FOUNDRY_API_KEY
POLICYTRACE_FOUNDRY_BEARER_TOKEN
```

Use **exactly one** authentication method: API key or bearer token.

### Git Bash example using an API key

```bash
export POLICYTRACE_FOUNDRY_ENDPOINT="https://policytrance.services.ai.azure.com"
export POLICYTRACE_FOUNDRY_MODEL="gpt-5-mini"
export POLICYTRACE_FOUNDRY_API_KEY="YOUR_KEY_HERE"
unset POLICYTRACE_FOUNDRY_BEARER_TOKEN

python backend/run_app.py
```

### Bearer-token form

```bash
export POLICYTRACE_FOUNDRY_ENDPOINT="https://policytrance.services.ai.azure.com"
export POLICYTRACE_FOUNDRY_MODEL="gpt-5-mini"
unset POLICYTRACE_FOUNDRY_API_KEY
export POLICYTRACE_FOUNDRY_BEARER_TOKEN="YOUR_TOKEN_HERE"

python backend/run_app.py
```

The contest rehearsal was validated with the **API-key** path. The bearer-token path is supported by the client but was not the primary live contest configuration.

## How the client authenticates

When an API key is configured, PolicyTrace sends:

```text
api-key: <key>
```

When a bearer token is configured, it sends:

```text
Authorization: Bearer <token>
```

The request body includes the selected model and requests structured JSON output.

## Security

Never commit a real Microsoft Foundry API key or bearer token to GitHub.

The repository intentionally contains the endpoint and model name because those are configuration identifiers, not authentication secrets. The secret credential should be entered at runtime or supplied through an environment variable.

If a credential has ever been pasted into a public repository, rotate it in Microsoft Foundry / Azure immediately.

## Live proof

The contest presentation includes a Microsoft Foundry Monitor screenshot showing real requests and token usage from the PolicyTrace development and rehearsal workload.

For the implementation, see:

- `backend/foundry_client.py`
- `backend/run_app.py`
- `frontend/app/`

For the full development history, see the original repository:

https://github.com/steveonw/microsoft-letastlator-thing
