# Atly Places — plugin and extension

Find places by what you actually want. Atly scores close to 2 million places against more than 1,400
specific intents — *work friendly*, *gluten free*, *great cappuccino*, *dog friendly*, *good for a
first date* — from what real reviewers say, and keeps the statements behind every score. Coverage is
densest in the United States, with pockets in Mexico, Israel and Thailand.

This repo packages Atly's public MCP server for the tools that can install it. The server itself is
at `https://agentic-api.atly.com/v2/mcp`.

## Install

Listed in Anthropic's Claude plugin directory (Claude Code, Cowork and the Claude apps) at
[claude.ai/directory/atly](https://claude.ai/directory/atly); or install from this repository as below.

**Claude Code**

```
/plugin marketplace add atlyai/atly-plugin
/plugin install atly-places@atly
```

Then `/atly-places:atly` in any session brings the skill in; the tools are there either way.

**Gemini CLI**

```
gemini extensions install https://github.com/atlyai/atly-plugin
```

**ChatGPT, Claude (chat), or anything else that speaks MCP** — add the server by URL:

```
https://agentic-api.atly.com/v2/mcp
```

It works without signing in; when the free allowance runs out, sign in with your email when
prompted and the connector then runs under your own account.

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

## What this plugin runs and sends

Nothing runs locally: the plugin is a skill (text) and the address of a remote MCP server. When the tools
are used, the agent sends `agentic-api.atly.com` what it needs to search: short search words taken from the
user's request (such as "gluten free" or "midtown"), an area name or the user's coordinates and a radius,
place ids, and, if it calls `submit_feedback`, one sentence about how a result went (the tool's description
asks for no user words or personal details there). The plugin adds nothing else and contacts no other
destination; the MCP client itself sends its usual connection details (client name, user-agent, and an
OAuth token if signed in). Atly logs each request's parameters, user-agent and IP address for quotas, abuse
prevention and product statistics; see the privacy policy below.

## Access and limits

No key or sign-in is needed to start; anonymous callers get a small quota. Over MCP (this plugin,
chat connectors) the way up is signing in with your email, which the client asks for when the
anonymous quota runs out (in Claude Code: `/mcp`, then Authenticate). For direct REST calls, `POST
https://agentic-api.atly.com/v2/keys` with an email returns a key with a daily quota of its own,
5,000 a day once the email is verified.

- Guide for agents: https://agentic-api.atly.com/v2/docs
- OpenAPI contract: https://agentic-api.atly.com/v2/openapi.yaml
- Coverage: densest in the United States, with pockets in Mexico, Israel and Thailand
- Privacy: https://www.atly.com/privacy · Terms: https://www.atly.com/terms

Built by [Atly](https://www.atly.com) (Steps Solutions Ltd.).
