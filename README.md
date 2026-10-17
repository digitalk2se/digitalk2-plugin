# DigitalK2 plugin

[![DigitalK2 MCP connector on Glama](https://glama.ai/mcp/connectors/se.digitalk2/digitalk2/badges/score.svg)](https://glama.ai/mcp/connectors/se.digitalk2/digitalk2)
[![MCP Registry](https://img.shields.io/badge/MCP_Registry-se.digitalk2%2Fdigitalk2-blue)](https://registry.modelcontextprotocol.io/v0/servers?search=digitalk2)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Swedish annual reports (årsredovisning) for small limited companies under K2, prepared with your AI in Claude, ChatGPT, Cursor or Codex.

[DigitalK2](https://www.digitalk2.se) is a hosted service for annual reports (årsredovisning) for small Swedish limited companies (aktiebolag) under the K2 rules. It produces the annual report in iXBRL, the tax calculation and the INK2 tax return, and files the report digitally with Bolagsverket. This repository connects AI agents to the DigitalK2 MCP server, so your AI can upload the SIE file, review the figures, read the tax calculation and fill in the report together with you.

It contains an MCP server configuration, plugin manifests for Claude Code, Cursor, Codex and the [`plugins`](https://www.npmjs.com/package/plugins) CLI, and a skill that tells the agent how to use the tools.

MCP server URL:

```
https://mcp.digitalk2.se/mcp
```

Sign-in happens with OAuth in your browser the first time the agent connects, with your DigitalK2 account. No API key is needed. You choose the permissions: **Read** (see drafts, figures, tax calculation) and **Prepare** (fill in and change drafts in steps 1 to 6).

## How it works

The AI works in the same guided flow as the web guide, with the same checks. You keep the draft open on digitalk2.se in AI mode next to the chat and see everything the AI fills in. Payment, personal identity numbers, signing and filing with Bolagsverket are always done by you on the web in step 7, never by the AI.

## Installation

### Plugin (Claude Code, Cursor, Codex and more)

```bash
npx plugins add digitalk2se/digitalk2-plugin
```

### Skill only

```bash
npx skills add digitalk2se/digitalk2-plugin
```

### Manual connection

- **Claude** (claude.ai and Claude Desktop): Settings → Connectors → Add custom connector, paste the URL and click Connect. In Claude Code:

  ```bash
  claude mcp add --transport http digitalk2 https://mcp.digitalk2.se/mcp
  ```

  Then type `/mcp` in Claude Code, choose `digitalk2` and sign in.

- **ChatGPT** (Go, Plus or Pro): open Plugins in the sidebar, click Add → Create custom MCP server, name it DigitalK2, paste the URL under Server URL, choose OAuth, tick "I understand and want to continue" and click Create as a plugin.

- **Cursor** (Pro or higher): add this to `~/.cursor/mcp.json`:

  ```json
  {
    "mcpServers": {
      "digitalk2": {
        "url": "https://mcp.digitalk2.se/mcp"
      }
    }
  }
  ```

- **Codex**:

  ```bash
  codex mcp add digitalk2 --url https://mcp.digitalk2.se/mcp
  ```

  Open a new Codex chat afterwards, since tools load when the session starts.

Step-by-step guides with animations: [www.digitalk2.se/mcp](https://www.digitalk2.se/mcp)

## Listed in

- [Official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=digitalk2) as `se.digitalk2/digitalk2` ([`server.json`](server.json))
- [Glama](https://glama.ai/mcp/connectors/se.digitalk2/digitalk2)

## Tools

| Tool | Access | What it does |
|---|---|---|
| `whoami` | Read | Shows the connected DigitalK2 account and its permissions. |
| `list_drafts` | Read | Lists the user's annual report drafts, most recently changed first. |
| `get_state` | Read | Shows where a draft is in the guide: current step, whether Next can be pressed, open questions and messages. |
| `read_draft` | Read | Reads the content of a draft one section at a time, as shown in the guide. |
| `read_tax` | Read | Reads the tax calculation and INK2 exactly as step 3 shows them. |
| `preview_report` | Read | Previews the full annual report as text, generated with the same code as the preview in step 7. |
| `create_draft` | Prepare | Creates a new, empty draft and returns the link to open it in AI mode. |
| `create_sie_upload` | Prepare | Creates a temporary upload link for a SIE file that is too large to send in a tool call. |
| `upload_sie` | Prepare | Uploads a SIE file (SIE4) to the draft and reads it in step 1. |
| `fill_input` | Prepare | Types into a form field in step 1 (company details). For figures and the sections in steps 2 to 6, use `update_fields`. |
| `update_fields` | Prepare | Fills in or changes fields in the current step in steps 2 to 6 (figures, tax, management report, notes, signatories). For the form fields in step 1, use `fill_input`. |
| `press_button` | Prepare | Presses a button or yes/no question in the form of the current step (listed in `actions` from `get_state`), like the user would. For dialogs the guide shows (listed in `prompts`), use `answer_prompt`. |
| `answer_prompt` | Prepare | Answers a dialog the guide shows (listed in `prompts` from `get_state` or a tool result) by pressing the same button the user would. For buttons and yes/no questions in the form itself, use `press_button`. |
| `next_step` | Prepare | Presses Next in the guide, with exactly the same checks as for the user. |
| `go_to_step` | Prepare | Goes back to an earlier step, like clicking the timeline. |

None of the tools can pay, sign, enter personal identity numbers or file anything. The AI only sees the signed-in user's own drafts.

## Privacy and terms

- Privacy policy: [www.digitalk2.se/integritet](https://www.digitalk2.se/integritet)
- Terms: [www.digitalk2.se/villkor](https://www.digitalk2.se/villkor)
- Contact: [hej@digitalk2.se](mailto:hej@digitalk2.se)

## På svenska

DigitalK2 är årsredovisning för små svenska aktiebolag (K2), som du kan göra tillsammans med din AI. Ladda upp SIE-filen från bokföringsprogrammet, granska resultat- och balansräkningen, få skatteberäkningen och INK2, och lämna in årsredovisningen digitalt (iXBRL) till Bolagsverket. Det här repot kopplar AI-agenter som Claude, ChatGPT, Cursor och Codex till DigitalK2:s MCP-server. Installera med `npx plugins add digitalk2se/digitalk2-plugin`, eller lägg till adressen `https://mcp.digitalk2.se/mcp` som anpassad koppling i din klient. Du loggar in med ditt DigitalK2-konto första gången. Betalning, signering och inlämning gör du alltid själv i steg 7. Mer information och guider finns på [www.digitalk2.se/mcp](https://www.digitalk2.se/mcp).

## License

The files in this repository are released under the [MIT License](LICENSE).

Configuration only. DigitalK2 is a proprietary hosted service.
