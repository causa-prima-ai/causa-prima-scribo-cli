<img width="3840" height="1920" alt="A4 _ CLI" src="https://github.com/user-attachments/assets/d002deb4-f136-47cd-9c52-8144a962c879" />

# Scribo CLI
> Free, AI-native e-invoicing — from your terminal.

## Contents

- [What is Scribo?](#what-is-scribo)
- [What is the Scribo CLI?](#what-is-the-scribo-cli)
  - [Demo](#demo)
- [Getting started](#getting-started)
  - [Quickstart](#quickstart)
  - [Setup & prerequisites](#setup--prerequisites)
- [Usage](#usage)
  - [How it works](#how-it-works)
  - [Examples](#examples)
- [Compliance](#compliance)
- [FAQ](#faq)
- [Resources](#resources)
- [License](#license)

## What is Scribo?

Scribo is a free, conversational e-invoicing tool. Describe an invoice in plain language — "bill Acme GmbH €2,400 for May design work" — and it drafts a structured invoice for you to review, download and send directly.

Just ask your AI agent about Scribo: it ships as an MCP server, a CLI, and a Claude/Codex skill, alongside a public HTTP API and a web app. Free forever — no credit card. You just need a sender email.

**Built by Causa Prima** — Scribo is built by [**Causa Prima**](https://causaprima.ai/), a company building agentic AI for the CFO office. It's our first freely available skill — an early glimpse of what we're building.

## What is the Scribo CLI?

The Scribo CLI brings compliant invoicing to your terminal. Install it from npm, run `scribo create …`, and get a finished invoice — a PDF with the matching e-invoice XML — written straight to a file. It wraps the public Scribo API with [sysexits](https://man.freebsd.org/cgi/man.cgi?sysexits)-style exit codes, so it drops cleanly into shell scripts and CI pipelines.

Free, no signup — your sender email is the login. Run it globally or one-off via `npx`.

## Getting started

### Quickstart

Run a one-off with `npx` (no install):

```sh
npx @causaprima/scribo-cli create \
  --jurisdiction DE --currency EUR \
  --sender-name "Example GmbH" --sender-country DE \
  --sender-address "Example Allee 1" --sender-postcode 10115 --sender-city Berlin \
  --sender-tax-id DE123456788 --sender-email billing@example.com \
  --recipient-name "Acme GmbH" --recipient-country DE \
  --recipient-address "Hauptstrasse 1" --recipient-postcode 10117 --recipient-city Berlin \
  --recipient-email ap@acme.example \
  --line "Senior consulting,3,1200,19,DAY,S" \
  --due-date 2026-06-01 \
  -o invoice.pdf
```

Output:

```
Invoice created
  invoice_id:  f0b3c1e2-...
  format:      zugferd_comfort
  download:    https://scribo.causaprima.ai/i/f0b3c1e2-.../download
  expires:     2099-12-31T23:59:59Z
  validator:   ok (invopop)
  saved to:    invoice.pdf
```

On the first invoice for a sender email the CLI prompts for the 6-digit verification code Scribo emails you (use `--no-prompt` in CI — it prints the challenge and exits `75`). The same email carries a magic link for signing back in and re-downloading.

### Setup & prerequisites

**Prerequisites:** Node.js 20 or later.

**Install** globally for repeated use:

```sh
npm install -g @causaprima/scribo-cli
scribo <command>
```

…or run any command one-off with `npx @causaprima/scribo-cli <command>`.

**Configuration** — override defaults via env vars (or the `--base-url` / `--api-key` flags):

| Env var | Purpose | Default |
|---|---|---|
| `SCRIBO_API_BASE_URL` | Public API origin (`SCRIBO_BASE_URL` accepted as fallback) | `https://scribo.causaprima.ai` |
| `SCRIBO_API_KEY` | Optional bearer token (partner quotas) | _(unset)_ |

**Verify:** `scribo jurisdictions` (add `--json` for machine-readable output) prints the supported-jurisdictions matrix.

## Usage

### How it works

Every command talks to the public Scribo API and follows BSD sysexits exit codes, so scripts can branch on `$?` without parsing output.

| Command | What it does |
|---|---|
| `scribo create` | Generate an invoice and save it locally (`-o file`). Build from flags, or pass a full `CreateInvoiceInput` with `--from invoice.json`. |
| `scribo get <invoiceId>` | Print invoice metadata as JSON (includes a fresh signed `download_url`). |
| `scribo download <invoiceId> [-o file]` | Stream the invoice bytes to a file. |
| `scribo jurisdictions` | Print the supported-jurisdictions matrix (`--json` for machine output). |

**Line items** repeat `--line` per row:

```
--line "<description>,<quantity>,<unit_price>,<tax_rate>[,<unit_code>[,<tax_category_code>[,<tax_exemption_code>]]]"
```

`unit_code` defaults to `EA`, `tax_category_code` to `S` (standard-rated). Escape commas in the description with a backslash. The 7th field, `tax_exemption_code`, is required when `tax_category_code=E` — pass a `VATEX-*` code matching the legal basis (see [tax codes](https://scribo.causaprima.ai/docs/tax-codes)).

**Payment details** — SEPA: `--payment-iban` (+ optional `--payment-bic`); US: `--payment-account-number` + `--payment-routing-number` (+ optional `--payment-bank`), instead of an IBAN — not both. XRechnung (German B2G) requires the IBAN (BR-DE-1).

**Exit codes** (BSD sysexits): `0` success · `64` bad usage · `65` 4xx (incl. an empty or rejected verification code) · `66` not found · `70` 5xx · `75` rate-limited / temporarily blocked / `--no-prompt` verification pending (retry) · `1` network/other.

Full flag reference: [scribo.causaprima.ai/docs/cli](https://scribo.causaprima.ai/docs/cli).

### Examples

**Kleinunternehmer (§ 19 UStG, VAT-exempt)**

```sh
scribo create \
  --sender-name "Friedrich Beratung" --sender-country DE \
  --sender-address "Musterstr. 1" --sender-postcode 10115 --sender-city Berlin \
  --sender-email f@example.de \
  --recipient-name "Acme GmbH" --recipient-country DE \
  --recipient-address "Hauptstr. 1" --recipient-postcode 10117 --recipient-city Berlin \
  --recipient-email ap@acme.de \
  --currency EUR \
  --line "Beratung,5,80.00,0,HUR,E,VATEX-EU-79-C"
```

**German B2G XRechnung (Leitweg-ID + IBAN)**

```sh
scribo create \
  --sender-name "Acme GmbH" --sender-country DE \
  --sender-address "Musterstr. 1" --sender-postcode 10115 --sender-city Berlin \
  --sender-tax-id DE123456788 --sender-email billing@acme.de \
  --sender-contact-name "Erika Beispiel" --sender-contact-phone "+49 30 1234567" \
  --recipient-name "Bundesamt für Beispiele" --recipient-country DE \
  --recipient-address "Wilhelmstr. 1" --recipient-postcode 10117 --recipient-city Berlin \
  --recipient-email rechnung@bund.de \
  --recipient-leitweg-id "991-12345-67" \
  --currency EUR \
  --payment-iban DE89370400440532013000 --payment-account-name "Acme GmbH" \
  --line "Consulting,3,1200.00,19,DAY,S"
```

**CI usage — branch on exit code**

```sh
scribo create --from invoice.json -o out.pdf || case $? in
  75) echo "rate limited; backing off"; exit 75 ;;
  70) echo "server error; retry later"; exit 1 ;;
  *)  exit 1 ;;
esac
```

## Compliance

Scribo emits invoices conforming to **EN 16931**, the European e-invoicing standard, with the relevant national CIUS. Every EN 16931 output (ZUGFeRD, XRechnung) is validated against the **Invopop**-hosted validator at generate-time — output that fails the rule set never reaches the user. US plain PDFs carry no EN 16931 XML and are rendered directly.

**Supported formats**

| Jurisdiction | Format | Status |
|---|---|---|
| Germany (B2B) | **ZUGFeRD** COMFORT (PDF/A-3 hybrid + CII XML) | ✅ Live |
| Germany (B2G) | **XRechnung** (UBL / CII) | ✅ Live |
| United States | Plain PDF (no XML, no e-invoice claim) | ✅ Live |
| France | **Factur-X** EN 16931 | 🔜 Coming soon |
| Spain | **Facturae** | 🔜 Coming soon |
| Belgium / NL / LU / AT | **Peppol BIS 3.0** UBL | 🔜 Coming soon |

*Disclaimer: Scribo generates and validates compliant invoice documents. It is **not tax or legal advice** — Scribo does not determine your tax obligations, VAT treatment, or filing requirements.*

## FAQ

<details>
<summary><strong>Is Scribo really free?</strong></summary>

Yes — free forever. No credit card, no subscription, no paywall before your first invoice. You just need a sender email.

</details>

<details>
<summary><strong>Do I need an account or signup?</strong></summary>

No signup form. On your first invoice, Scribo verifies the sender email — a 6-digit code (or one-click link) arrives at that address; one verification covers ~30 minutes of invoicing. The same email doubles as your magic-link login for re-downloads later.

</details>

<details>
<summary><strong>Which countries and formats are supported?</strong></summary>

Live today: **Germany** — ZUGFeRD (B2B) and XRechnung (B2G) — and the **United States** (plain PDF). Coming next: **France** (Factur-X), **Spain** (Facturae), and **Belgium / NL / LU / AT** (Peppol BIS 3.0).

</details>

<details>
<summary><strong>What does "EN 16931-compliant" actually mean here?</strong></summary>

Every EN 16931 output (ZUGFeRD, XRechnung) is validated against the **EN 16931-1:2017** rule set (via the Invopop-hosted validator) before it's returned. Output that fails validation never reaches you. US plain PDFs carry no EN 16931 XML, so no schematron validation applies.

</details>

<details>
<summary><strong>Is the US version compliant?</strong></summary>

Yes. There is no US e-invoicing mandate, so Scribo produces a fully compliant plain PDF.

</details>

<details>
<summary><strong>Does Scribo give tax or legal advice?</strong></summary>

No. Scribo generates and validates compliant invoice *documents*. It does not determine your tax obligations, VAT treatment, or filing requirements.

</details>

<details>
<summary><strong>How do AI agents / LLMs use Scribo?</strong></summary>

Scribo is built to be operated by an agent. It ships as an **MCP server**, a **CLI**, and a **Claude/Codex skill**, plus a public **HTTP API** and a **web app** — all on the same backend. An agent can discover Scribo, create an invoice, and return the file on a user's behalf.

</details>

<details>
<summary><strong>Who builds Scribo?</strong></summary>

[Causa Prima](https://causaprima.ai/) — a company building agentic AI for the CFO office. Operated by Causa Prima Germany GmbH, Munich.

</details>

## Resources

- **Web app** — [scribo.causaprima.ai](https://scribo.causaprima.ai)
- **Documentation** — [scribo.causaprima.ai/docs](https://scribo.causaprima.ai/docs)
- **Compliance & trust** — [scribo.causaprima.ai/compliance](https://scribo.causaprima.ai/compliance)
- **Causa Prima** — [causaprima.ai](https://causaprima.ai)

**Other Scribo surfaces**

- Skill (Claude / Codex) — [`scribo-skill`](https://github.com/causa-prima-ai/scribo-skill) · [docs](https://scribo.causaprima.ai/docs/skill)
- MCP server — [`scribo-mcp`](https://github.com/causa-prima-ai/scribo-mcp) · [docs](https://scribo.causaprima.ai/docs/mcp)
- CLI — [`scribo-cli`](https://github.com/causa-prima-ai/scribo-cli) · [docs](https://scribo.causaprima.ai/docs/cli)
- HTTP API — [`scribo-api-docs`](https://github.com/causa-prima-ai/scribo-api-docs) · [docs](https://scribo.causaprima.ai/docs/api)
- Brand hub — [`scribo`](https://github.com/causa-prima-ai/scribo)

## License

Proprietary — `UNLICENSED`. © Causa Prima Germany GmbH. All rights reserved. Distributed for use against the public Scribo API; not open-source. See [LICENSE](./LICENSE).

