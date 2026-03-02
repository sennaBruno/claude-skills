---
name: evolution-api-management
description: Use when deploying, connecting, troubleshooting, or managing Evolution API WhatsApp instances on ECS Fargate. Triggers on QR code generation, WhatsApp session conflicts, webhook JWT errors, instance lifecycle operations, or environment isolation for WhatsApp services.
---

# Evolution API Management

## Overview

Operational guide for Evolution API v2.3.7 on ECS Fargate with WhatsApp Baileys integration. Covers instance lifecycle, session management, environment isolation, and troubleshooting.

**Core constraint:** WhatsApp allows only ONE active Baileys connection per phone number. Two instances with the same number = continuous `conflict/replaced` loop every ~2 seconds.

## Quick Reference

| Operation | Endpoint | Method |
|-----------|----------|--------|
| List instances | `/instance/fetchInstances` | GET |
| Create instance | `/instance/create` | POST |
| Connect (QR) | `/instance/connect/{name}` | GET |
| Connection state | `/instance/connectionState/{name}` | GET |
| Logout | `/instance/logout/{name}` | DELETE |
| Delete instance | `/instance/delete/{name}` | DELETE |
| Set webhook | `/webhook/set/{name}` | POST |

All endpoints require header: `apikey: $AUTHENTICATION_API_KEY`

## ECS Exec Commands

Container is BusyBox-based: **no curl/wget**. Use `node -e` for everything.

```bash
# Template for all ECS exec commands
aws ecs execute-command \
  --cluster "$CLUSTER" \
  --task "$TASK_ID" \
  --container EvolutionService \
  --interactive \
  --command 'node -e "...script..."'
```

**Get task ID:**
```bash
TASK_ID=$(aws ecs list-tasks --cluster "$CLUSTER" --service-name EvolutionService \
  --query 'taskArns[0]' --output text | awk -F/ '{print $NF}')
```

**ECS exec output gotchas:**
- Output always contains `Starting session...` and `Cannot perform start session: EOF` noise
- Use `grep` or `tail` to extract useful data
- Long outputs get truncated (~3KB). For large data: write to file inside container, read in chunks via base64
- Shell escaping through ECS exec is fragile. For complex payloads: base64-encode the JSON body, decode inside the node script

## Instance Lifecycle

### Create Instance with Webhook

```javascript
// Body for POST /instance/create
{
  instanceName: 'alinhou-production',
  integration: 'WHATSAPP-BAILEYS',
  qrcode: true,
  rejectCall: false,
  groupsIgnore: true,
  alwaysOnline: false,
  readMessages: false,
  readStatus: false,
  syncFullHistory: false,
  webhook: {
    url: 'https://lambda-url/api/v1/webhook/evolution',
    byEvents: true,
    base64: false,
    events: ['MESSAGES_UPSERT'],
    headers: { jwt_key: 'YOUR_WEBHOOK_SECRET' }  // MUST have a value
  }
}
```

### Update Webhook on Existing Instance

```javascript
// POST /webhook/set/{instanceName}
// MUST wrap in "webhook" property
{
  webhook: {
    enabled: true,
    url: 'https://lambda-url/api/v1/webhook/evolution',
    webhookByEvents: true,
    webhookBase64: false,
    events: ['MESSAGES_UPSERT'],
    headers: { jwt_key: 'YOUR_WEBHOOK_SECRET' }
  }
}
```

### QR Code Generation Workflow

1. Call `/instance/connect/{name}` - returns `{ code, base64, pairingCode }`
2. The `code` field is the raw QR data string (~240 chars)
3. Generate QR locally: `npx qrcode -o /tmp/qr.png "$CODE"` or use `require('qrcode').toDataURL()`
4. Create HTML with embedded base64 image (no CDN dependency)
5. Copy to Windows: `/mnt/c/Users/{user}/Downloads/`
6. QR expires in ~45 seconds - be fast

**Never try to pipe base64 image data through ECS exec** - it gets truncated and corrupted.

## Environment Isolation

### The Problem

WhatsApp Baileys protocol enforces single connection per phone number. Staging + production competing = session conflict loop.

### The Solution

| Component | Staging | Production |
|-----------|---------|------------|
| `WHATSAPP_MOCK` | `'true'` | `'false'` |
| `EVOLUTION_INSTANCE_NAME` | `alinhou-staging` | `alinhou-production` |
| ECS scaling | `{ min: 0, max: 0 }` | `{ min: 1, max: 1 }` |
| Evolution DB | `/evolution` | `/evolution_prod` |
| Outage checker | Returns mock stub | Real Evolution calls |

**In SST infra:**
```typescript
WHATSAPP_MOCK: $app.stage === 'production' ? 'false' : 'true',
EVOLUTION_INSTANCE_NAME: `alinhou-${$app.stage}`,
scaling: $app.stage === 'production' ? { min: 1, max: 1 } : { min: 0, max: 0 },
```

### Outage Checker Mock Guard

When `WHATSAPP_MOCK=true`, `getEvolutionStatus()` must return early with a healthy stub. Otherwise, it tries to reach a non-existent Evolution container and triggers false-positive outage alert emails after 30 minutes.

## Webhook JWT Configuration

- `jwt_key` goes inside `headers` object in the webhook config
- Evolution strips `jwt_key`, generates HS256 JWT with `{ app: "evolution", action: "webhook" }`
- JWT has 10-minute expiry, sent as `Authorization: Bearer <token>`
- Lambda verifies using `EVOLUTION_WEBHOOK_SECRET` env var
- **`EVOLUTION_WEBHOOK_SECRET` is NOT in the Evolution container env** - only in Lambda. When creating instances via ECS exec, you must pass the secret value directly (get from `npx sst secret list`)
- Missing JWT key causes: `secretOrPrivateKey must have a value` error in Evolution logs

## Session Persistence

Dual strategy for maximum resilience:
- **Database**: PostgreSQL `Session` table (when `DATABASE_SAVE_DATA_INSTANCE=true`)
- **EFS**: Mounted at `/evolution/instances/` for local Baileys auth files

Either alone is sufficient. Both together = defense in depth.

## Deploy Strategy (Stop-First)

```typescript
// In infra/evolution.ts transform
deploymentMaximumPercent: 100,
deploymentMinimumHealthyPercent: 0,
```

Old task stops before new starts. ~3-5 min downtime acceptable:
- Outbound messages queue in SQS
- Inbound messages buffered by WhatsApp servers

## Brazilian Phone Normalization

WhatsApp sends 12-digit format: `558188101603` (drops mobile 9th digit)
Database stores 13-digit format: `5581988101603` (with the 9)

Webhook normalizes: if 12 digits starting with `55`, insert `9` at position 4.

## Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| `conflict/replaced` loop every ~2s | Two instances sharing same phone | Scale one environment to 0 |
| `secretOrPrivateKey must have a value` | JWT key empty in webhook config | Update webhook with `POST /webhook/set/{name}` and actual secret value |
| Instance stuck in `connecting` | Session conflict or stale auth | Logout, wait 5s, reconnect with fresh QR |
| 403 on instance create | Instance name already exists | Delete old instance first |
| QR scan says "cannot connect device" | Session conflict with another environment | Ensure only one environment has Evolution running |
| Messages not delivered | Phone number format mismatch | Check 12→13 digit normalization in webhook |
| Zombie instance (DB=open, WS=dead) | Container restart without clean disconnect | Delete from DB, clear EFS, force new deployment, recreate instance |

## Container Details

- Image: `evoapicloud/evolution-api:v2.3.7` (don't upgrade without staging test)
- BusyBox-based: no curl, no wget, use `node -e`
- `startPeriod: 180s` (container + EFS mount + Prisma + Baileys reconnect)
- Health check: node HTTP GET to localhost:8080
- `LOG_LEVEL: 'ERROR,WARN,WEBHOOKS'` for clean CloudWatch

## Cost

Staging Evolution running 24/7 (0.25 vCPU, 1GB ARM64, sa-east-1): ~$8.51/month.
Scaling to 0 with mock mode saves this entirely.
