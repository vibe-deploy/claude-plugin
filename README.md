# Flitzy plugin for Claude Code

Deploy and manage apps on [Flitzy](https://flitzy.app) from Claude Code: `https://<name>.flitzy.app` with HTTPS, logs, databases and secrets.

```
/plugin marketplace add vibe-deploy/claude-plugin
/plugin install flitzy@flitzy
```

Then run `/mcp`, pick **flitzy** → Authenticate, and click **Allow** in the browser (sign in with your Flitzy account, pick the team). Ask: “Deploy this with Flitzy.”

What it adds:

- the hosted Flitzy MCP server `https://mcp.flitzy.app/mcp` (deploy folders through a one-time upload command, deploy from GitHub, status, logs, env vars, databases, vault secrets),
- the skill `deploy-with-flitzy`, which walks Claude through deploying and fixing an app until it is live.

Docs: https://docs.flitzy.app/start/agents/

This repository is generated from `claude-plugin/` in the Flitzy monorepo; changes made here are overwritten.
