# Claude Code Command Reference

Below is a clean, easy‑to‑read guide to the most useful slash commands you can run in the Claude Code interface. Each entry explains what the command does and when you might want to use it.

| Command        | Description                                                                                                                  | Typical Use‑Case                                                                                 |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **`/help`**    | Displays a help panel with all available commands and brief usage notes.                                                     | You’re unsure what commands exist or need a quick reminder.                                      |
| **`/clear`**   | Erases the entire chat history **and** the internal conversation context, giving you a brand‑new session.                    | Starting a completely unrelated discussion or discarding sensitive information.                  |
| **`/reset`**   | Keeps the visible chat history but wipes the internal context, so subsequent messages are interpreted without prior context. | You want to preserve the transcript for reference but need a fresh “mindset” for the next steps. |
| **`/compact`** | Compresses the stored context to save tokens while retaining the most relevant information.                                  | Your session is getting large and you’re approaching token limits.                               |
| **`/context`** | Shows the current conversation context, including any key variables or files that Claude is tracking.                        | Diagnosing why Claude is behaving a certain way or verifying what data it has loaded.            |
| **`/export`**  | Saves the full conversation (messages and context) to a file on disk for later review.                                       | Creating a record of the session for documentation, sharing with teammates, or future reference. |
| **`/config`**  | Opens the configuration view where you can read or modify Claude Code settings (e.g., model choice, permissions, hooks).     | Tweaking behavior, enabling/disabling permissions, or adjusting performance options.             |
| **`/model`**   | Shows the language model currently in use and lets you switch to a different one (e.g., Opus, Sonnet, Haiku).                | Trying a faster model for quick tasks or a more capable one for complex reasoning.               |
| **`/agents`** | Shows the list of available sub‑agents and their status. | Inspecting or debugging sub‑agents. |

| **`/permissions`** | Displays the current permissions granted to Claude Code and allows you to revoke or grant new ones. | Managing access to files, APIs, or other resources that Claude can interact with. |

### How to Run a Command

1. Type the slash (`/`) followed by the command name in the chat input.
2. Press **Enter**.
3. Claude Code will execute the command and reply with the result or prompt for any additional input.

Feel free to copy‑paste any of these commands directly into the chat to see them in action!
