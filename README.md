# Pi agent configuration

Personal Pi configuration intended to live at `~/.pi`.

## What's included

- `agent/settings.json` — preferences, extension package declarations, and the default model.
- `agent/mcp-adapter.json` — Blender MCP server configuration.
- `agent/skills/` — custom skills and their supporting files.
- `.gitignore` — rules that keep credentials and generated runtime files out of Git.

## Set up a new device

### 1. Install Pi

Install Pi using its official installation instructions. The Pi executable and managed installation are not stored in this repository.

### 2. Clone this repository

```sh
git clone git@github.com:CWZMorro/Pi-Agent.git ~/.pi
```

### 3. Install the extension packages

`agent/settings.json` records the desired packages, but their downloaded files are ignored. Use Pi's package manager to install them on the new device:

```sh
pi install npm:pi-venice
pi install npm:pi-resource-center
pi install npm:pi-web-access
pi install npm:pi-subagents
pi install npm:pi-mcp-adapter
pi install npm:@abhishek944/pi-image-gen
pi install npm:@juicesharp/rpiv-ask-user-question
pi install npm:@juicesharp/rpiv-todo
```

These commands reflect the current package declarations. If `agent/settings.json` changes, use its `packages` list as the source of truth. No package versions are pinned here, so installations on different devices may use different versions.

### 4. Authenticate providers

Start Pi and run:

```text
/login
```

Authenticate OpenAI to use the configured default model. Configure credentials for any other providers or extensions you use as well. API-key environment variables can be used where supported.

Credentials are device-local and must not be committed. The default model must be available to your provider account; if it isn't, select an available model with `/model`.

### 5. Configure Blender integration

Before using Blender tools:

- Install Blender and the required Blender MCP integration.
- Review `agent/mcp-adapter.json` for device-specific executable paths and environment settings, especially `BLENDER_BINARY`.
- Follow `agent/skills/qwen-mm-plugins-blender/SKILL.md` for prerequisites and usage.

The MCP configuration alone does not install Blender or its integration.

### 6. Verify the setup

Start a new Pi session and check:

- `/model` shows GPT-6.1 Sol as the selected model, if available.
- Your extensions and custom skills load without errors.
- Provider authentication works.
- Blender tools connect when you need them.

Use `/reload` after manually changing settings or resources. Resumed sessions can retain their previous model rather than adopting the startup default.

## What's intentionally excluded

The `.gitignore` excludes:

- Provider credentials (`agent/auth.json`) and local `.env` files.
- Conversation history (`agent/sessions/`).
- Pi installation files and generated launchers.
- Downloaded extension packages and `node_modules`.
- Generated model catalogs, MCP caches, and onboarding state.
- Subagent runtime state.
- Logs, temporary files, language caches, and OS metadata.

These files are either private, device-specific, or recreated locally. Cloning this repository does **not** restore session history or authentication.
