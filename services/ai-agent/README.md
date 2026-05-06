# Openclaw AI Agent Integration Notes

## Scope
- Customer care in WhatsApp.
- Guided ordering through chat.
- Order tracking and payment instructions.
- Human handoff for low confidence intents.

## Runtime Contract
1. Receive message event from `/whatsapp/webhook`.
2. Resolve customer + active order context.
3. Send prompt to Openclaw with tool definitions:
   - `search_menu`
   - `create_draft_order`
   - `confirm_order`
   - `track_order`
   - `handoff_human`
4. Persist action logs and outgoing message.

## Guardrails
- Never finalize payment state from chat only; require backend verification.
- Always confirm items + quantity + total before submit.
- Escalate after 2 failed intent resolutions.
