# Installation

## Node.js / TypeScript

```bash
npm install @novu/api
# or
pnpm add @novu/api
# or
yarn add @novu/api
```

## Environment Variables

```bash
NOVU_SECRET_KEY=your-secret-key-here
```

Get your API key from [dashboard.novu.co/api-keys](https://dashboard.novu.co/api-keys).

## Initialize the SDK

```typescript
import { Novu } from "@novu/api";

const novu = new Novu({
  security: { secretKey: process.env.NOVU_SECRET_KEY },
});
```

## Python

```bash
pip install novu-py
```

```python
from novu_py import Novu
import os

novu = Novu(
    security=Security(secret_key=os.environ["NOVU_SECRET_KEY"]),
)
```

## cURL

All API requests require the `Authorization` header:

```bash
curl -X POST https://api.novu.co/v1/events/trigger \
  -H "Authorization: ApiKey YOUR_SECRET_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "workflow-id",
    "to": "subscriber-id",
    "payload": {}
  }'
```

**Note:** In the REST API, the trigger field is called `name` (not `workflowId`). The SDK maps `workflowId` to `name` automatically.
