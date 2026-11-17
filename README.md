# 🤖 CartVai AI Assistant — ChatGPT & Codex Plugin

Official OpenAI Agent Plugin for [CartVai](https://cartvai.com) E-commerce SaaS. Connects ChatGPT and OpenAI Codex directly to a merchant's CartVai store via the Model Context Protocol (MCP).

---

## 🌟 Overview

The **CartVai AI Assistant** empowers merchants to monitor sales, review pending and unverified orders, track inventory, and manage product catalog drafts directly within ChatGPT or Codex using natural language (Bangla or English).

- **Native Protocol:** Model Context Protocol (MCP) over Streamable HTTP (`https://app.cartvai.com/api/mcp`).
- **Authentication:** Per-merchant Bearer API Token (`cv_live_sk_...`) generated in the CartVai Control Panel.
- **Tenant Isolation:** Hard, database-enforced multi-tenant isolation. No store ID parameter is accepted from the model.
- **Permissions:** Scoped access via `mcp:read` (Read-only) and `mcp:write` (Read + Write operations).

---

## 🛠️ Supported Tools (13 Tools)

### 📊 Store & Business Overview

- `get-store-summary` — Real-time summary of today's sales, order volume, pending orders, and low-stock alerts.

### 📦 Order Operations

- `list-orders` — Filter orders by status, date range, or customer name/phone. Phone numbers are automatically masked (`017••••321`) for privacy.
- `get-order` — Full details for a single order including line items, delivery address, COD verification status, and courier tracking (Steadfast, Pathao, RedX).
- `get-order-counts` — Aggregated order counts grouped by status.
- `list-missing-orders` — Identifies recoverable abandoned checkout sessions.
- `update-order-status` _(requires `mcp:write`)_ — Safely transitions order statuses following valid lifecycle rules.

### 🏷️ Catalog & Inventory

- `list-products` — Search and filter products by title, SKU, or status.
- `get-product` — Detailed product view with variants, pricing, and available stock.
- `list-low-stock` _(requires inventory plan)_ — Identifies out-of-stock items or products at/below low-stock thresholds.
- `create-product` _(requires `mcp:write`)_ — Adds simple physical product drafts with initial stock.
- `update-product` _(requires `mcp:write`)_ — Modifies title, regular price, sale price, SKU, or status.
- `adjust-stock` _(requires `mcp:write` + inventory plan)_ — Atomic inventory adjustment (`set`, `add`, `remove`) with row-level locking.

### 👥 Customer Insights

- `search-customers` — Search customer profiles with lifetime order count and total spend.

---

## 🔐 Security & Safety Architecture

1. **Token-Bound Tenant Context:** The authenticated merchant's API key strictly binds the session to their store. Cross-store access is physically impossible.
2. **Strict Guard Rails:** Write operations mandate explicit merchant confirmation before execution.
3. **Prompt Injection Defense:** Customer notes, delivery addresses, and customer names are treated as raw string data and never interpreted as prompt instructions.
4. **Instant Revocation:** Keys can be instantly revoked from the CartVai Control Panel (**Settings → Developer**).

---

## 📦 Package Structure

```text
plugins/cartvai-ai-assistant/
├── .codex-plugin/
│   └── plugin.json       # Codex/ChatGPT UI & presentation manifest
├── .mcp.json             # MCP server configuration with Bearer auth scheme
├── mcp.json              # Public MCP manifest
├── plugin.json           # Agent Plugins v1.0.0 root manifest
├── README.md             # Plugin documentation
└── skills/
    └── cartvai-merchant-assistant/
        └── SKILL.md      # Assistant persona, guard rails, and execution rules
```

---

## 🚀 How to Publish to OpenAI Plugin Marketplace

1. Visit the [OpenAI Developer Dashboard](https://developers.openai.com/plugins/build/plugins).
2. Go to **Plugins → Submit a Plugin** (or Upload Plugin Package).
3. Zip the plugin directory:
    ```bash
    cd plugins/cartvai-ai-assistant
    zip -r cartvai-ai-assistant.zip . -x "*.DS_Store"
    ```
4. Upload `cartvai-ai-assistant.zip` or link the repository path.
5. Provide developer contact info, privacy policy URL (`https://cartvai.com/privacy`), and submit for review.

---

## 📄 License & Attribution

Developed by the [CartVai](https://cartvai.com) Team.
For support, contact support@cartvai.com.
