# Setup Claude Code with Ollama (Windows)

## 1. Install Claude Code CLI
```powershell
irm https://claude.ai/install.ps1 | iex
```
*Reference:* https://code.claude.com/docs/en/quickstart

## 2. Install Ollama
```powershell
irm https://ollama.com/install.ps1 | iex
```
*Reference:* https://ollama.com/

## 3. Add Claude to your PATH
Add `C:\Users\agpas\.local\bin` to the **User** environment variable `Path`.

## 4. Run the tools
Open a PowerShell window and execute:

1. `ollama` – starts the Ollama service.
2. Launch Claude Code (`claude-code` or via the start menu).
3. In Claude Code, select the desired local model (e.g., `gemma4`).

> **Note:** Running models locally is slower than the hosted version. 
