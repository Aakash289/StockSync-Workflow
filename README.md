# StockSync-Workflow

Automated inventory reconciliation and low-stock reordering with a single n8n workflow.
Keeps Shopify (incl. Instagram Shop), your POS, and your warehouse DB perfectly in sync—and pings suppliers the moment stock dips below a threshold.

✨ What this is

A drop-in n8n workflow that:

Reads source-of-truth on-hand from your warehouse DB

Updates Shopify inventory levels (also syncs Instagram Shop via the Shopify sales channel)

(Optional) Updates POS inventory via API

Detects low stock and auto-notifies suppliers via Slack/email with a suggested reorder qty

🚩 Problem it solves

Fashion startups juggle inventory across Shopify, POS, and warehouse spreadsheets. Manual reconciliation causes stockouts/overselling and late reorders.
StockSync makes the warehouse the source of truth and keeps every channel aligned—in real time (webhooks) and on a schedule (cron).

🧭 How it works (flow)

Trigger

Webhooks: Shopify or POS events start an immediate sync

Cron: every 10 minutes, a full reconciliation runs

Fetch

Query Postgres for per-SKU on-hand + system mappings

Reconcile (Function node)

Push on-hand to Shopify (and optionally POS)

Mark OOS when qty = 0 (assumes inventory policy = deny)

If on-hand ≤ threshold → send Slack/email reorder alerts

🧩 Nodes

Cron: periodic sync (10m) — safety net reconciliation

Webhook: Shopify (inventory update) — real-time trigger from Shopify

Webhook: POS (sale/adjustment) — real-time trigger from POS

Postgres: fetch inventory + mappings (Execute Query) — reads warehouse_inventory + inventory_map

Function: reconcile + push + notify — updates Shopify/POS, enforces OOS, sends low-stock alerts
