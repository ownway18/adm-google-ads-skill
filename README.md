# ADM Google Ads Skill

An Agent Skill and remote MCP server that let Claude Code, Codex, Cursor and Windsurf read your Google Ads accounts, analyze performance and draft Search campaigns, through [ADM](https://adm.cc).

The skill is one file, [`skills/adm-google-ads/SKILL.md`](skills/adm-google-ads/SKILL.md), that describes the workflow. The work itself runs through the ADM MCP server, which connects your AI tool to the Google Ads accounts linked to your ADM account.

- MCP server: `https://app.adm.cc/mcp` (Streamable HTTP)
- Authentication: `Authorization: Bearer <YOUR_ADM_API_KEY>`
- Documentation: https://adm.cc/docs/google-ads-skill

## What it does

- **Reports**: campaigns, ad groups, keywords with Quality Score, responsive search ads with ad strength, assets and conversion actions, over the last 7, 14 or 30 days.
- **Search terms**: the queries that triggered your ads, with the matched keyword and performance.
- **Device and location performance**: performance split by device, country, region or city, with the current bid adjustments.
- **Negative keyword checks**: positive keywords blocked by negatives at account, shared list, campaign or ad group level.
- **AI campaign drafts**: from a landing page, ADM drafts a Search campaign or ad groups, with keywords grouped by theme and responsive search ads for each group.
- **Build plans**: turns a written plan, spreadsheet or Markdown file into a structured Google Ads build plan.

Read tools work on every ADM plan, including Free. Write access depends on your ADM plan and API key; see the [documentation](https://adm.cc/docs/google-ads-skill) for the current tool list. Every change is previewed first and applied only after your approval, and new campaigns are created paused.

## Installation

### 1. Create an API key

Sign in to ADM and create a key at https://app.adm.cc/apikeys. Store it in the `ADM_API_KEY` environment variable rather than in project files.

- macOS or Linux: add `export ADM_API_KEY="adm_..."` to `~/.zshrc` or `~/.bashrc`
- Windows: `setx ADM_API_KEY "adm_..."`, then open a new terminal

### 2. Connect the MCP server

**Claude Code**

```bash
claude mcp add --transport http --scope user adm https://app.adm.cc/mcp --header "Authorization: Bearer <YOUR_ADM_API_KEY>"
```

Check the connection with `claude mcp list`.

**Codex**

```bash
codex mcp add adm --url https://app.adm.cc/mcp --bearer-token-env-var ADM_API_KEY
```

**Cursor** (`~/.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "adm": {
      "url": "https://app.adm.cc/mcp",
      "headers": { "Authorization": "Bearer ${env:ADM_API_KEY}" }
    }
  }
}
```

**Windsurf** (`~/.codeium/windsurf/mcp_config.json`)

```json
{
  "mcpServers": {
    "adm": {
      "serverUrl": "https://app.adm.cc/mcp",
      "headers": { "Authorization": "Bearer ${env:ADM_API_KEY}" }
    }
  }
}
```

### 3. Install the skill

**Claude Code plugin**

```bash
claude plugin marketplace add ownway18/adm-google-ads-skill
claude plugin install adm-google-ads@adm-google-ads-skill
```

**Manual copy**: copy the `skills/adm-google-ads` folder into the skills directory of your AI tool. The folder name must stay `adm-google-ads`.

| AI tool | Path |
|---|---|
| Claude Code | `~/.claude/skills/adm-google-ads/SKILL.md` |
| Codex | `.agents/skills/adm-google-ads/SKILL.md` |
| Cursor | `.cursor/skills/adm-google-ads/SKILL.md` |
| Windsurf | `.windsurf/skills/adm-google-ads/SKILL.md` |

**Let the AI tool install it**: send this prompt, with `adm_xxx` replaced by your key:

```text
Install the ADM MCP server and the ADM Google Ads skill by following https://adm.cc/docs/google-ads-skill#ai-install. My ADM API key is adm_xxx
```

Restart the AI tool after installation. The latest skill file is always available at https://adm.cc/docs/google-ads-skill/SKILL.md.

## Example prompts

- "Show last week's Google Ads performance by campaign."
- "Which search terms spent money without converting in the last 30 days?"
- "Are any of my keywords blocked by negative keywords?"
- "Draft a Search campaign for https://example.com/pricing."

## Links

- ADM: https://adm.cc
- Skill and MCP documentation: https://adm.cc/docs/google-ads-skill
- API keys: https://app.adm.cc/apikeys

## License

[MIT](LICENSE)
