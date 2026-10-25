# Solk MCP Server

**Official remote MCP server for [Solk](https://solk.app), the B2B sales CRM for field sales teams.**

Connect Claude, Claude Code, ChatGPT developer mode, Cursor, VS Code or any MCP client to your Solk workspace. Ask about accounts and deals in plain language, prepare for meetings, review the pipeline, log visits and calls, build quotes and orders, and check ERP balances — always with your own Solk permissions.

```text
https://mcp.solk.app/mcp
```

| | |
|---|---|
| Transport | Streamable HTTP (remote, hosted) |
| Auth | OAuth 2.1 — PKCE S256, client ID metadata documents and dynamic client registration |
| Scopes | `crm.read`, `crm.write` (+ optional `crm.delete`, unticked by default) |
| Tools | 100+ (57 read, 41 write, 10 optional delete) |
| Prompts | 11 ready-made prompts (account brief, meeting prep, deal review, weekly pipeline, build a quote …) |
| Resources | workspace schema, sales process, record and canvas summaries |
| Registry name | `app.solk/solk` |
| Docs | [docs.solk.app/en/mcp/overview](https://docs.solk.app/en/mcp/overview) |

> This repository holds documentation and the MCP Registry metadata (`server.json`). The server itself is hosted by Solk — there is nothing to install or run locally.

## What you can ask

- "Who handles purchasing at Kuzey Plastik, and what did we last talk about?"
- "What are my overdue actions for today and this week?"
- "Sort the deals in Negotiation by value and flag the ones past their SLA."
- "Log the call I just had with Alize Paketleme and create a quote follow-up for Thursday."
- "Build a quote for 2 tonnes of B-20 tape at our contract price and send it for approval."
- "What is this customer's overdue balance in the ERP, and how close are they to their credit limit?"
- "Draw our quote approval process as a flowchart on a new canvas."

## Connect

### Claude (web, desktop, mobile)

1. **Customize → Connectors → + Add → Add custom connector**
2. Name `Solk`, URL `https://mcp.solk.app/mcp` — leave the advanced client ID fields empty.
3. Click **Connect**, enter your workspace (`yourcompany` or `yourcompany.solk.app`), sign in and approve the permissions.

On Team and Enterprise plans an organization owner adds the connector once; each member then connects with their own Solk account.

### Claude Code

```bash
claude mcp add --transport http solk https://mcp.solk.app/mcp
```

Then run `/mcp`, choose **solk** and approve access in the browser window that opens.

### Cursor, VS Code, ChatGPT developer mode and other clients

Add `https://mcp.solk.app/mcp` as a remote (Streamable HTTP) MCP server. Clients that support MCP authorization discover OAuth on their own.

```json
{
  "mcpServers": {
    "solk": {
      "url": "https://mcp.solk.app/mcp"
    }
  }
}
```

For tools that only accept a static header, a Solk admin can create an API key in **Settings → Developers** and send `Authorization: Bearer sk_…` (the key acts with that admin's permissions).

Every workspace also has its own address, `https://<company>.solk.app/mcp`, with the same permissions.

## Tools

| Area | Examples |
|---|---|
| Search and records | `search`, `list_companies`, `get_company`, `list_contacts`, `get_opportunity`, `get_relations` |
| Daily work | `get_today`, `list_actions`, `complete_action`, `postpone_action`, `list_calendar`, `create_calendar_event` |
| Pipeline | `list_opportunities`, `create_opportunity`, `change_opportunity_stage` (pipeline gates apply), `run_report`, `aggregate` |
| Activity | `log_activity`, `search_activities`, `add_note`, `add_comment`, `list_call_recordings` |
| Quote to cash | `list_products`, `create_offer`, `update_offer_status`, `create_order`, `get_company_finance`, `list_contracts` |
| Support and automation | `list_tickets`, `create_ticket`, `update_ticket`, `list_automations`, `run_workflow` |
| E-mail | `list_email_templates`, `render_email_template`, `create_email_draft` (drafts only — nothing is sent) |
| Canvases | `create_canvas` (templates or Mermaid flowcharts), `read_canvas`, `update_canvas` |

Full list and parameters: [docs.solk.app/en/mcp/tools](https://docs.solk.app/en/mcp/tools)

## Security

- **OAuth, not passwords** — the client never sees your password; the consent screen lists the permissions it asks for.
- **Your permissions, nothing more** — role, module access and record visibility apply exactly as in the app.
- **Read, write and delete are separate** — read tools are marked read-only; permanent delete and merge need the separate `crm.delete` permission and `confirm: true` on every call.
- **Revocable and audited** — see connected apps, last use and call counts in **Settings → App connections**, remove access instantly, and find every tool call in the audit log.
- **The gateway stores nothing** — `mcp.solk.app` only routes requests; codes, tokens and record data stay in your own workspace.

Limits: 300 requests per minute per token; tool responses are capped at about 90,000 characters.

## About Solk

Solk is a closed-loop CRM for B2B field sales teams at manufacturers, distributors and industrial suppliers: visit logging with location check-in, trade-fair lead capture, gated pipeline stages, quotes with discount approval, orders and shipment planning, built-in AI agents, call summaries, and integrations with Logo, Netsis, Mikro, Paraşüt, SAP Business One, e-invoice providers, phone systems, SMS and WhatsApp Business. Web, iOS and Android. 14-day free trial.

- Website: [solk.app](https://solk.app)
- Docs: [docs.solk.app](https://docs.solk.app) · [llms.txt](https://docs.solk.app/llms.txt)
- Privacy: [solk.app/en/privacy.html](https://solk.app/en/privacy.html) · Terms: [solk.app/en/terms.html](https://solk.app/en/terms.html)
- Support: [solk.app/en/support.html](https://solk.app/en/support.html) · launch@solk.app
