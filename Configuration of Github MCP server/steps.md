# GitHub MCP Server Setup for Claude Code

> A quick‑start guide that works for both **Claude Code (CLI)** and **Claude Desktop**. Follow the steps in order; each one is self‑contained and includes the necessary commands.

---

## 1️⃣ Prerequisites

| Item | Action |
|------|--------|
| **GitHub account** | Any free or paid account works. |
| **Personal Access Token (PAT)** | <ol><li>Visit <https://github.com/settings/tokens> → **Generate new token (classic)**.</li><li>Give it a clear name, e.g., `Claude MCP`.</li><li>Enable the minimal scopes you need (see table below).</li><li>Copy the token – you’ll only see it once.</li></ol> |
| **Claude Code CLI** | Install per the official docs (`brew install anthropic/cli/claude-code` on macOS, the Windows installer, or the Linux package). |
| **Docker** *(only for the local server)* | Install Docker Desktop (<https://www.docker.com/products/docker-desktop>) and ensure the daemon is running. |
| **Optional – .env** | Store the token securely: `echo "GITHUB_PAT=ghp_…yourtoken…" > .env` and add `.env` to `.gitignore`. |

### Minimum token scopes

- `repo` – read/write access to repositories, issues, and pull requests.  
- `read:org` – read‑only access to organization data (needed for org‑wide repos).  
- *(Add `gist` only if you need Gist support.)*

---

## 2️⃣ Remote HTTP Server (no Docker required)

> Works with **Claude Code ≥ 2.1.1**. Older CLI versions can use the *legacy* syntax shown later.

### 2️⃣ A. Register the server

```bash
# If you stored the token in .env:
PAT=$(grep GITHUB_PAT .env | cut -d= -f2)

claude mcp add-json github \
  "{\"type\":\"http\",\"url\":\"https://api.githubcopilot.com/mcp\",\"headers\":{\"Authorization\":\"Bearer $PAT\"}}" \
  --scope user   # use "local" for project‑only, "user" for a global config
```

*The JSON payload describes a **http**‑type MCP server and injects the `Authorization` header.*

### 2️⃣ B. Verify the registration

```bash
claude mcp list          # should list `github   ✔   http`
claude mcp get github    # prints the JSON you just added
```

### 2️⃣ C. Test a prompt

Enter Claude Code and ask, e.g.:

```
> List my GitHub repositories
```

Claude should return a nicely formatted list of repositories you have access to.

---

## 3️⃣ Local Docker Server (full control, works for Claude Desktop too)

> Use this when you need a custom toolset, want to run against GitHub Enterprise, or cannot reach the remote HTTP endpoint.

### 3️⃣ A. Pull the official image (optional)

```bash
docker pull ghcr.io/github/github-mcp-server
```

Claude Code will pull the image automatically on first use, but pulling ahead of time avoids a delay.

### 3️⃣ B. Register the server via CLI

```bash
# Substitute the token (or pull it from .env as above)
PAT=$(grep GITHUB_PAT .env | cut -d= -f2)

claude mcp add github \
  -e GITHUB_PERSONAL_ACCESS_TOKEN=$PAT \
  -- \
  docker run -i --rm \
    -e GITHUB_PERSONAL_ACCESS_TOKEN \
    ghcr.io/github/github-mcp-server
```

**Explanation**

| Flag | Meaning |
|------|---------|
| `add github` | Name of the server (you’ll refer to it as `github`). |
| `-e GITHUB_PERSONAL_ACCESS_TOKEN=…` | Pass the PAT to Claude Code’s environment so it can forward it to Docker. |
| `-- docker …` | Instruct Claude Code to launch the server with the given Docker command. |

### 3️⃣ C. Verify (same as remote)

```bash
claude mcp list
claude mcp get github
```

You should see the server type reported as `command` (Docker) and a green check mark.

### 3️⃣ D. Use it in Claude Code

```
> Show open issues in anthropic/claude
```

Claude will query the locally‑running Docker server and list the issues.

### 3️⃣ E. (Optional) Run the binary directly – no Docker

1. Download the latest release from the GitHub MCP repo’s *Releases* page (e.g., `github-mcp-server_1.0.3_linux_amd64.tar.gz`).
2. Extract and place the binary on your `$PATH`.
3. Register with:

```bash
claude mcp add-json github \
  '{"command":"github-mcp-server","args":["stdio"],"env":{"GITHUB_PERSONAL_ACCESS_TOKEN":"YOUR_PAT"}}' \
  --scope user
```

---

## 4️⃣ Claude Desktop Configuration (Docker method only)

Claude Desktop does not support the **http** server type (OAuth would be required). Use the Docker method.

### 4️⃣ A. Locate the Desktop config file

| OS | Path |
|----|------|
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |
| Linux | `~/.config/Claude/claude_desktop_config.json` |

### 4️⃣ B. Insert the server definition (replace `YOUR_PAT`)

```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "GITHUB_PERSONAL_ACCESS_TOKEN",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "YOUR_PAT"
      }
    }
  }
}
```

*If you launch Claude Desktop from a shell that already exported `GITHUB_PERSONAL_ACCESS_TOKEN`, you can omit the `env` block and just keep the `-e` flag.*

### 4️⃣ C. Restart Claude Desktop

Quit the app completely, then start it again. The server will start automatically when you enter **Agent mode** (the Copilot‑style chat).

### 4️⃣ D. Verify in the UI

- Open **Settings → Connectors** – you should see “GitHub” with a green check.
- Or open the log file (see the troubleshooting table below) and look for `Server started on stdio`.

### 4️⃣ E. Test a prompt in Desktop

```
> List my GitHub repos
```

---

## 5️⃣ Common Troubleshooting

| Symptom | Quick Fix |
|---------|-----------|
| `claude mcp list` shows “failed” | 1️⃣ Ensure Docker daemon is running (`docker ps`).<br>2️⃣ Re‑run the `add` command with a fresh PAT.<br>3️⃣ Restart Claude Code or Desktop. |
| Authentication error | Verify the PAT includes the `repo` (and `read:org` if needed) scope. Regenerate the token if unsure. |
| Windows JSON‑escaping errors (legacy CLI) | Use the *legacy* syntax:
```
claude mcp add --transport http github https://api.githubcopilot.com/mcp -H "Authorization: Bearer $PAT"
```
| Docker pull fails (`ghcr.io` auth) | Run `docker logout ghcr.io` then retry the pull. |
| Server not appearing in Desktop UI | Validate the JSON in `claude_desktop_config.json` with a linter, then restart the app. |
| “I can’t access GitHub right now” | Check network connectivity, token expiration, and that the URL `https://api.githubcopilot.com/mcp` is reachable. |

**Log locations for deeper inspection**

| Platform | Path |
|----------|------|
| Claude Code (CLI) | Run `/mcp` in the terminal – it prints status and recent logs. |
| Claude Desktop (macOS) | `~/Library/Logs/Claude/mcp-server-*.log` |
| Claude Desktop (Windows) | `%APPDATA%\Claude\logs\mcp-server-*.log` |

---

## 6️⃣ Security Best Practices

1. **Never commit the PAT** – keep it in `.env` (git‑ignored) or an OS env var.
2. **Limit token scopes** to the minimum required (see the table above).
3. **Rotate tokens** regularly (every 30‑60 days for production).
4. **Add `.env` (or `.mcp.json`) to `.gitignore`** to avoid accidental leaks.
5. For **GitHub Enterprise** deployments, set the `GITHUB_HOST` env var or pass `--gh-host` to the Docker run command.

---

## 7️⃣ One‑Line Cheat Sheets

### Remote HTTP (quick copy‑paste)
```bash
PAT=$(grep GITHUB_PAT .env | cut -d= -f2) && \
claude mcp add-json github "{\"type\":\"http\",\"url\":\"https://api.githubcopilot.com/mcp\",\"headers\":{\"Authorization\":\"Bearer $PAT\"}}" --scope user && \
claude mcp list
```

### Local Docker (quick copy‑paste)
```bash
PAT=$(grep GITHUB_PAT .env | cut -d= -f2) && \
claude mcp add github -e GITHUB_PERSONAL_ACCESS_TOKEN=$PAT -- \
  docker run -i --rm -e GITHUB_PERSONAL_ACCESS_TOKEN ghcr.io/github/github-mcp-server && \
claude mcp list
```

---

*That’s everything you need to get Claude Code (or Claude Desktop) talking to GitHub via the MCP server.*
