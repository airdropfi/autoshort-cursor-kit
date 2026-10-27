# AutoShort for Cursor and Grok Build

Plan, draft and render faceless short videos on your own [AutoShort](https://autoshort.org) account from Cursor or Grok Build. You review the script first, and nothing that costs credits runs until you have seen the price AutoShort returns and said yes.

[![Add AutoShort to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/link/mcp/install?name=autoshort&config=eyJ1cmwiOiJodHRwczovL2F1dG9zaG9ydC5vcmcvYXBpL21jcCJ9)

The button opens Cursor and asks you to confirm adding the `autoshort` MCP server. Prefer the app link? `cursor://anysphere.cursor-deeplink/mcp/install?name=autoshort&config=eyJ1cmwiOiJodHRwczovL2F1dG9zaG9ydC5vcmcvYXBpL21jcCJ9`

## What it contains

| Component | Name | What it does |
|---|---|---|
| MCP server | `autoshort` (`https://autoshort.org/api/mcp`) | The AutoShort tools. Sign-in is OAuth (authorization code with PKCE); there is no key to paste. |
| Rule | `rules/autoshort.mdc` | Plan before you produce, price first, no income claims, content limits. |
| Skill | `make-a-faceless-short` | Idea, script review, free draft, price check, render after your yes. |
| Skill | `plan-a-series` | Season plan for a multi-episode story: save, review, confirm, then produce. |
| Skill | `research-winning-formats` | Read your own performance and the template gallery, then get a test plan. |

## Install in Cursor

1. Click the button above, or add this to `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

   ```json
   {
     "mcpServers": {
       "autoshort": {
         "url": "https://autoshort.org/api/mcp"
       }
     }
   }
   ```

2. Open Customize, then MCPs. Next to `autoshort`, sign in. A browser window opens: sign in to AutoShort and click Allow.
3. Optional, the rule: copy `rules/autoshort.mdc` to `.cursor/rules/autoshort.mdc` in your project. It is set to "Agent Decides", so Cursor applies it when you ask for a video.
4. Optional, the skills: copy the folders in `skills/` to `~/.cursor/skills/`.

Cursor asks you to approve MCP tool calls by default. Leave that on: rendering spends credits.

## Install in Grok Build

```bash
grok mcp add --transport http autoshort https://autoshort.org/api/mcp
```

or in `~/.grok/config.toml`:

```toml
[mcp_servers.autoshort]
url = "https://autoshort.org/api/mcp"
```

Grok Build loads `.cursor/mcp.json` too. Then open `/mcps`, select `autoshort` and press `i` to sign in.

## Use

Try the prompts in [examples/prompts.md](examples/prompts.md). A typical session:

1. "Make a 30-second short about why octopuses have three hearts." The agent writes the script as a table and waits.
2. You edit it. The agent saves a free draft.
3. The agent asks AutoShort for the price and your balance and says them back to you.
4. Only after you say yes does it render, then it gives you the link when the video is done.

## What it will and will not do

- A draft is free and renders nothing. Every paid step (a render, a character sheet, an episode) is priced by AutoShort first and needs your explicit yes.
- It never buys credits and never turns on auto-recharge. To add credits you use AutoShort's own checkout page.
- Narration is read exactly as written. AutoShort does not rewrite it.
- Real or famous people, protected characters and sexual content are refused.
- Videos use AI-generated images and voice. Switch on the platform's AI-content label when you post.
- No result is promised: AutoShort cannot tell you how a video will perform.

## Data and privacy

The MCP server receives what the tools need: the scripts, briefs and settings you ask your agent to send, and the video, credit and series data on your AutoShort account. This repository stores nothing and runs no code on your machine; it is a server address, Markdown instructions and a logo. See the [privacy policy](https://autoshort.org/en/privacy) and [terms](https://autoshort.org/en/terms). Sign-in tokens are revocable in your AutoShort dashboard under API keys.

## Layout

```text
.cursor-plugin/plugin.json   Cursor Plugin manifest (also read by Grok Build)
mcp.json                     MCP server for Cursor
.mcp.json                    the same server for Grok Build and other loaders
rules/autoshort.mdc          rule
skills/                      three skills (SKILL.md)
assets/logo.png              1024x1024 logo
examples/prompts.md          example prompts
```

## Support

https://autoshort.org/en/support or support@autoshort.org. Documentation: https://autoshort.org/en/developers.

## License

MIT for the files in this repository (see `LICENSE`). The AutoShort service is governed by its terms.
