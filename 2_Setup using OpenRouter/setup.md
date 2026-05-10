# Claude Setup Guide (Windows)

## Prerequisites
- **Windows PowerShell** (default on Windows 10/11)
- An **OpenRouter API key** (you’ll generate this in step 3)

---

## 1️⃣ Install Claude

Open PowerShell **as Administrator** and run the official installer script:

```powershell
irm https://claude.ai/install.ps1 | iex
```

The script downloads Claude and places the executable in `C:\Users\<your‑username>\.local\bin`.

---

## 2️⃣ Add Claude to Your `PATH`

1. Open **System Settings** → **Advanced system settings** → **Environment Variables**.
2. Under **User variables**, locate **Path** and click **Edit**.
3. Add the folder:

```
C:\Users\<your‑username>\.local\bin
```
4. Click **OK** on all dialogs, then restart any open terminals to apply the change.

> **Tip:** Verify the installation by running `claude --version` in a new PowerShell window.

---

## 3️⃣ Generate an OpenRouter API Key

1. Log in to your OpenRouter account.
2. Navigate to the **API Keys** section.
3. Click **Create New Key**, give it a descriptive name (e.g., “Claude‑CLI”), and copy the generated key.

> **Keep this key secret** – treat it like a password.

---

## 4️⃣ Configure Claude to Use OpenRouter

Create or edit the Claude configuration file at:

```
C:\Users\<your‑username>\.claude\settings.json
```

Paste the following JSON, replacing the placeholder with the API key you just generated and selecting a model from the OpenRouter Models tab:

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://openrouter.ai/api",
    "ANTHROPIC_AUTH_TOKEN": "<your-openrouter-api-key>",
    "ANTHROPIC_API_KEY": "",          // leave empty
    "ANTHROPIC_MODEL": ""             // e.g., "anthropic/claude-3-opus-20240229"
  }
}
```

- **`ANTHROPIC_AUTH_TOKEN`** – your OpenRouter API key.
- **`ANTHROPIC_MODEL`** – the model you wish to use (choose a free tier model on the OpenRouter **Models** page).

Save the file.

---

## 5️⃣ Verify the Setup

Open a new PowerShell window and run a quick test:

```powershell
claude "Hello, Claude! Please confirm you’re connected to OpenRouter."
```

You should see a response from Claude confirming the connection and model name.

---

## 📌 Summary Checklist

- [ ] Install Claude via PowerShell script.
- [ ] Add `C:\Users\<username>\.local\bin` to the user `PATH`.
- [ ] Generate an OpenRouter API key.
- [ ] Populate `settings.json` with the API key and chosen model.
- [ ] Run a test query to confirm everything works.

You’re now ready to use Claude on Windows with OpenRouter as the backend!