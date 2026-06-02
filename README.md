<img width="3840" height="1920" alt="A4 _ CLI" src="https://github.com/user-attachments/assets/d002deb4-f136-47cd-9c52-8144a962c879" />

# Scribo CLI
> Free, AI-native e-invoicing — from your terminal.

## Contents

- [What is Scribo?](#what-is-scribo)
- [What is the Scribo CLI?](#what-is-the-scribo-cli)
- [Compliance](#compliance)
- [FAQ](#faq)
- [Resources](#resources)
- [License](#license)
- [Previous README](#previous-readme)

## What is Scribo?

Scribo is a free, conversational e-invoicing tool. Describe an invoice in plain language — "bill Acme GmbH €2,400 for May design work" — and it drafts a structured invoice for you to review, download and send directly.

Just ask your AI agent about Scribo: it ships as an MCP server, a CLI, and a Claude/Codex skill, alongside a public HTTP API and a web app. Free forever — no credit card. You just need a sender email.

**Built by Causa Prima** — Scribo is built by [**Causa Prima**](https://causaprima.ai/), a company building agentic AI for the CFO office. It's our first freely available skill — an early glimpse of what we're building.

## What is the Scribo CLI?

> 🚧 **Status: planned.** The Scribo CLI hasn't shipped yet — star or watch this repo to be notified. Getting started and Usage sections land when the CLI ships. In the meantime, see **[Previous README](#previous-readme)** below for the surface map, or reach for the [skill](https://github.com/causa-prima-ai/causa-prima-scribo-skill), [MCP server](https://github.com/causa-prima-ai/causa-prima-scribo-mcp), or [HTTP API](https://github.com/causa-prima-ai/causa-prima-scribo-api-docs) today.

## Compliance

Scribo emits invoices conforming to **EN 16931**, the European e-invoicing standard, with the relevant national CIUS. Every invoice is validated against the **Invopop**-hosted EN 16931 validator at generate-time — output that fails the rule set never reaches the user.

**Supported formats**

| Jurisdiction | Format | Status |
|---|---|---|
| Germany (B2B) | **ZUGFeRD** COMFORT (PDF/A-3 hybrid + CII XML) | ✅ Live |
| Germany (B2G) | **XRechnung** (UBL / CII) | ✅ Live |
| United States | Plain PDF (no XML, no e-invoice claim) | ✅ Live |
| France | **Factur-X** EN 16931 | 🔜 Coming soon |
| Spain | **Facturae** | 🔜 Coming soon |
| Belgium | **Peppol BIS 3.0** UBL | 🔜 Coming soon |

*Disclaimer: Scribo generates and validates compliant invoice documents. It is **not tax or legal advice** — Scribo does not determine your tax obligations, VAT treatment, or filing requirements.*

## FAQ

**Is Scribo really free?**
Yes — free forever. No credit card, no subscription, no paywall before your first invoice. You just need a sender email.

**Do I need an account or signup?**
No signup form. Scribo uses a magic-link login: you provide a sender email (which the invoice needs anyway), confirm via a one-time link, and you're in.

**Which countries and formats are supported?**
Live today: **Germany** — ZUGFeRD (B2B) and XRechnung (B2G) — and the **United States** (plain PDF). Coming next: **France** (Factur-X), **Spain** (Facturae), and **Belgium** (Peppol BIS 3.0).

**What does "EN 16931-compliant" actually mean here?**
Every invoice is validated against the **EN 16931-1:2017** rule set (via the Invopop-hosted validator) before it's returned. Output that fails validation never reaches you.

**Is the US version compliant?**
Yes. There is no US e-invoicing mandate, so Scribo produces a fully compliant plain PDF.

**Does Scribo give tax or legal advice?**
No. Scribo generates and validates compliant invoice *documents*. It does not determine your tax obligations, VAT treatment, or filing requirements.

**How do AI agents / LLMs use Scribo?**
Scribo is built to be operated by an agent. It ships as an **MCP server**, a **CLI**, and a **Claude/Codex skill**, plus a public **HTTP API** and a **web app** — all on the same backend. An agent can discover Scribo, create an invoice, and return the file on a user's behalf.

**Who builds Scribo?**
[Causa Prima](https://causaprima.ai/) — a company building agentic AI for the CFO office. Operated by Causa Prima Germany GmbH, Munich.

## Resources

- **Web app** — [scribo.causaprima.ai](https://scribo.causaprima.ai)
- **Documentation** — [scribo.causaprima.ai/docs](https://scribo.causaprima.ai/docs)
- **Compliance & trust** — [scribo.causaprima.ai/compliance](https://scribo.causaprima.ai/compliance)
- **Causa Prima** — [causaprima.ai](https://causaprima.ai)

**Other Scribo surfaces**

- Skill (Claude / Codex) — [`causa-prima-scribo-skill`](https://github.com/causa-prima-ai/causa-prima-scribo-skill) · [docs](https://scribo.causaprima.ai/docs/skill)
- MCP server — [`causa-prima-scribo-mcp`](https://github.com/causa-prima-ai/causa-prima-scribo-mcp) · [docs](https://scribo.causaprima.ai/docs/mcp)
- CLI — [`causa-prima-scribo-cli`](https://github.com/causa-prima-ai/causa-prima-scribo-cli) · [docs](https://scribo.causaprima.ai/docs/cli)
- HTTP API — [`causa-prima-scribo-api-docs`](https://github.com/causa-prima-ai/causa-prima-scribo-api-docs) · [docs](https://scribo.causaprima.ai/docs/api)
- Brand hub — [`causa-prima-scribo`](https://github.com/causa-prima-ai/causa-prima-scribo)

## License

Proprietary — `UNLICENSED`. © Causa Prima Germany GmbH. All rights reserved. Distributed for use against the public Scribo API; not open-source. See [LICENSE](./LICENSE).

---

## Previous README

> **Note:** This repo's README is being migrated to Scribo's shared structure (consistent title, shared sections, and per-surface sections). The content below is the previous README, preserved verbatim — minus the header visual, which now sits at the top of this page.

<details>
<summary>Show previous README</summary>

# Scribo CLI

> 🚧 **Status: planned.** This repo is the placeholder for the official Scribo command-line interface. Star or watch this repo to be notified when it ships.

For what Scribo is and the full surface map, see the [umbrella repo](https://github.com/causa-prima-ai/causa-prima-scribo).

## Other ways to use Scribo today

- **Claude Desktop / Cursor / Cline / ChatGPT App** → [`scribo-mcp`](https://github.com/causa-prima-ai/causa-prima-scribo-mcp)
- **Claude Code / Codex CLI** → [`scribo-skill`](https://github.com/causa-prima-ai/causa-prima-scribo-skill)
- **HTTP API directly** → [`scribo-api-docs`](https://github.com/causa-prima-ai/causa-prima-scribo-api-docs)
- **Web app** → [scribo.causaprima.ai](https://scribo.causaprima.ai)
- **Umbrella** → [`scribo`](https://github.com/causa-prima-ai/causa-prima-scribo)

## License

Proprietary — © Causa Prima Germany GmbH. All rights reserved. See [LICENSE](./LICENSE).

</details>