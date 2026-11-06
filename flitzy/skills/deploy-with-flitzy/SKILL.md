---
name: deploy-with-flitzy
description: Deploy the current project (or a GitHub repository) to Flitzy and fix it until it is live. Use when the user asks to deploy, host, publish or put an app online with Flitzy, or asks why their Flitzy app is down.
---

# Deploy with Flitzy

Flitzy hosts apps at `https://<name>.flitzy.app`. Use the `flitzy` MCP tools.

1. **Check the app can run on Flitzy.** It must listen on `0.0.0.0` and the port in `$PORT` (default 8080). Stacks are detected without a Dockerfile: Node (`start` script = server, only `build` = static site), Python (requirements.txt / pyproject + Procfile or main.py / app.py), Go (go.mod), plain `index.html`. Fix the code first if needed.
2. **Pick the name.** Use `"name"` from `flitzy.json` if the project has one, else a short lowercase form of the folder name.
3. **Deploy.**
   - Local folder: call `deploy_app` with `name` and the absolute `path`. It returns an upload command: run it with the Bash tool exactly as given (it packs the folder without `node_modules`, `.git` and `.env` files and sends it). It prints JSON with an `id`.
   - GitHub repository: call `deploy_from_github` with `repo` (and `path` for a folder in a monorepo). Private repos need a GitHub token in the team vault as `GITHUB_TOKEN`; ask the user to create one with read access and store it with `set_secret` - never invent one.
4. **Wait.** Call `wait_for_deploy` with the id until it says live or failed.
5. **On failure**, read the error and logs, fix the cause in the code, and deploy again. For a running app that crashes, use `app_status` and `app_logs`.
6. **State that must survive deploys** (SQLite, uploads): pass `volume: true` and write to `$DATA_DIR` (`/data`). For PostgreSQL or Redis, `create_database` and pass it in `databases`.
7. Secrets go in `env` (or the vault with `set_secret` + `secrets`), never in code or `flitzy.json`.

Never delete apps, volumes, databases or secrets unless the user asked for exactly that. When a limit error carries an upgrade link, show it to the user.
