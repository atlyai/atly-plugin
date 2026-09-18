# Atly Places — plugin and extension

Find places by what you actually want. Atly scores close to 2 million places against more than 1,400
specific intents — *work friendly*, *gluten free*, *great cappuccino*, *dog friendly*, *good for a
first date* — from what real reviewers say, and keeps the statements behind every score. Coverage is
densest in the United States, with pockets in Mexico, Israel and Thailand.

This repo packages Atly's public MCP server for the tools that can install it. The server itself is
at `https://agentic-api.atly.com/mcp`.

## Install

**Claude Code**

```
/plugin marketplace add atlyai/atly-plugin
/plugin install atly-places@atly
```

Then `/atly` in any session brings the skill in; the tools are there either way.

**Gemini CLI**

```
gemini extensions install https://github.com/atlyai/atly-plugin
```

**ChatGPT, Claude (chat), or anything else that speaks MCP** — add the server by URL:

```
https://agentic-api.atly.com/mcp
```

Authorize with your email when prompted; the connector then runs under your own account.

## What you can ask

- Find a work-friendly cafe with great cappuccino in New York.
- Where can I get a safe gluten-free dinner in Los Angeles?
- A dog-friendly spot with good coffee and outdoor seating in San Francisco.
- Best coffee within 2 km of 40.7580, -73.9855.

Answers come back ranked, with a 0–10 score per requested intent, quotable statements from real
reviews, and a link to each place.

## What's in here

| | |
|---|---|
| `.claude-plugin/` | Claude Code plugin + marketplace manifests |
| `skills/atly/SKILL.md` | how to use the tools well — the workflow, and the traps |
| `.mcp.json` | the remote MCP server, for Claude Code |
| `gemini-extension.json`, `GEMINI.md` | the same for Gemini CLI |

## Access and limits

No key is needed to start; anonymous callers share a small hourly quota per IP. `POST
https://agentic-api.atly.com/v0/keys` with an email returns a key with a daily quota, and verifying
that email raises it. Chat connectors authorize with OAuth instead and need no key at all.

- Guide for agents: https://agentic-api.atly.com/v0/docs
- OpenAPI contract: https://agentic-api.atly.com/v0/openapi.yaml
- Coverage: United States
- Privacy: https://www.atly.com/privacy · Terms: https://www.atly.com/terms

Built by [Atly](https://www.atly.com) (Steps Solutions Ltd.).
