# Instagram Account Management

Agent workspace for managing Meta (Instagram, Facebook, WhatsApp) integrations
through Meta's official remote MCP servers.

## MCP servers

Configured in [`.mcp.json`](.mcp.json) and picked up automatically by Claude Code
when you open this project.

| Server | URL | Purpose |
|---|---|---|
| `meta_social_technologies` | `https://mcp.facebook.com/devtools` | Manage Meta apps on developers.facebook.com: webhooks, compliance, App Review status, API health, docs search |
| `whatsapp_business_tools` | `https://mcp.facebook.com/whatsapp_business_tools` | WhatsApp Business Platform: accounts, phone numbers, message templates, webhooks, sending messages |

Docs: [Meta MCP overview](https://developers.facebook.com/documentation/mcp) ·
[Social Technologies MCP](https://developers.facebook.com/documentation/mcp/devtools-mcp) ·
[WhatsApp Business Tools MCP](https://developers.facebook.com/documentation/mcp/whatsapp-business-tools-mcp)

## Setup

1. Open the project in Claude Code (`claude` in this directory) and approve the
   project MCP servers when prompted.
2. Run `/mcp`, pick each server and sign in with your Meta account (OAuth).
3. On the consent screen, select the apps to grant. Keep each app at **Read**
   scope unless an agent actually needs to make changes; adjust later under
   facebook.com → Settings → [Business Integrations](https://www.facebook.com/settings/?tab=business_tools).
4. Verify: ask Claude "what Meta tools do you have?"

Equivalent manual setup (or for other clients):

```bash
claude mcp add --transport http meta_social_technologies https://mcp.facebook.com/devtools
claude mcp add --transport http whatsapp_business_tools https://mcp.facebook.com/whatsapp_business_tools
```

Other validated clients: Claude Desktop, ChatGPT (Web), Codex App/CLI, Cursor App/CLI.

## Permissions

[`.claude/settings.json`](.claude/settings.json) splits the tools in two:

- **Read-only tools are auto-allowed:** listing apps, App Review/compliance
  status, API usage, webhook lists, docs search, WhatsApp
  businesses/accounts/numbers/templates.
- **Tools that change state or send things always ask first:** webhook
  create/update/delete/test, phone-number onboarding, template
  create/update/delete, sending messages, payments, business verification,
  and system-user tokens.

## Security

Using these servers binds you (and the agent) to the
[Meta Platform Terms](https://developers.facebook.com/terms), including data
retention and use limits on anything returned by Meta APIs.

- Agents can call anything your granted scopes allow, including when told to by
  instructions hidden in untrusted content (webhook payloads, documents, web
  pages, other tool output). Don't give Manage scope to agents that read
  untrusted input.
- Use separate Meta apps for dev and prod; never grant production scopes to
  experimental agents.
- Review connected integrations under Business Integrations every few months and
  revoke unused ones.
