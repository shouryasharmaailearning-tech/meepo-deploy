# meepo-deploy

Everything needed to run [meepo](https://github.com/shouryasharmaailearning-tech/meepo)
via its published Docker image — no cloning the source, no build step.

## Quick start

```bash
cp .env.example .env            # fill in whatever you want (all optional)
docker compose run --rm meepo db upgrade
docker compose run --rm meepo sync-config --file feeds.example.yaml
docker compose up -d web         # http://localhost:8000
docker compose run --rm meepo run --since 1d
```

## What's here

- `docker-compose.yml` — pulls `ghcr.io/shouryasharmaailearning-tech/meepo:v0.1.0`,
  runs two services: `meepo` (one-shot `fetch`/`digest`/`send`/`run`, via
  `docker compose run`) and `web` (the configuration UI on `:8000`). Both share the
  same data volume, so settings saved through the UI apply to both.
- `.env.example` — every optional setting (Discord webhook, LLM provider + API key).
  Copy to `.env`; `docker compose` reads it automatically.
- `feeds.example.yaml` — a starting feed list (Hacker News, Python Insider). Load it
  with `sync-config` as shown above, or just add feeds through the web UI instead.

## Summaries

Defaults to a local [Ollama](https://ollama.com) server reached via
`host.docker.internal:11434` (set up automatically in `docker-compose.yml` for Linux
too, via `extra_hosts`) — install Ollama on the host and `ollama pull llama3.2:3b`, no
API key needed. To use Anthropic or OpenAI instead, set `MEEPO_LLM_PROVIDER` and the
matching key in `.env`, or switch it on the web UI's Settings page at any time —
whichever you set last wins (the UI's value overrides `.env`).

## Updating to a newer release

```bash
docker compose pull
docker compose up -d web
```

Full source, issues, and the project's design notes live in the main repo:
https://github.com/shouryasharmaailearning-tech/meepo
