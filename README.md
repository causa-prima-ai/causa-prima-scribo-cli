# Scribo CLI

> 🚧 **Status: planned.** This repo is the placeholder for the official Scribo command-line interface. Star or watch this repo to be notified when it ships.

For what Scribo is and the full surface map, see the [umbrella repo](https://github.com/causa-prima-ai/scribo).

## What it will be

A small npm package (working name `@causa-prima/scribo`) that wraps the public [Scribo HTTP API](https://github.com/causa-prima-ai/scribo-api-docs) so you can generate an EN 16931-compliant invoice from a shell pipeline, a Makefile, or a CI job.

```sh
npx @causa-prima/scribo \
  --from "Acme GmbH" \
  --to "Globex Ltd" \
  --amount 1200 \
  --jurisdiction DE \
  > invoice.pdf
```

Same backend as every other Scribo surface — your first invoice's sender email becomes your login, then you can re-download from [scribo.causaprima.ai](https://scribo.causaprima.ai).

## Other ways to use Scribo today

- **Claude Desktop / Cursor / Cline / ChatGPT App** → [`scribo-mcp`](https://github.com/causa-prima-ai/scribo-mcp)
- **Claude Code / Codex CLI** → [`scribo-skill`](https://github.com/causa-prima-ai/scribo-skill)
- **HTTP API directly** → [`scribo-api-docs`](https://github.com/causa-prima-ai/scribo-api-docs)
- **Web app** → [scribo.causaprima.ai](https://scribo.causaprima.ai)
- **Umbrella** → [`scribo`](https://github.com/causa-prima-ai/scribo)

## License

MIT.
