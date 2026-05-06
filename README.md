# AI-Menu Kitchen Commerce Starter

This repository now contains a starter implementation derived from `docs/kitchen-app-blueprint.md`.

## What is included
- `apps/public-store/index.html`: mobile-first QR storefront with section pricing and WhatsApp order handoff.
- `apps/admin/index.html`: admin shell for quick-start actions and key modules.
- `backend/api-spec.yaml`: initial OpenAPI surface for products, sales, customers, credits, table sections, WhatsApp, and AI agent endpoints.
- `services/ai-agent/README.md`: Openclaw integration contract and guardrails.

## Run locally
Use any static file server, for example:

```bash
python3 -m http.server 8080
```

Then open:
- Admin: `http://localhost:8080/apps/admin/`
- Public store sample:
  `http://localhost:8080/apps/public-store/?section=VIP&adjType=percent&adjVal=10&wa=2348012345678`
