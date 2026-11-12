# Skaler MCP server

Meta ads creative analytics, competitor ad research and ad launching for Claude, ChatGPT and other MCP clients.

[Skaler](https://skaler.app) is AI creative analytics for performance marketers, DTC brands and agencies running Meta ads. It tags every ad by hook, angle, format and persona and shows which creatives drive ROAS and CPA. The MCP server gives your AI assistant the same data, plus competitor ad research and a launcher that creates new ad tests, paused by default.

> **This repository holds documentation and configuration only.** The Skaler MCP server is a hosted service and its source code is not public. There is nothing to install or run from here: you connect to the hosted endpoint.

| | |
|---|---|
| Server URL | `https://skaler.app/api/mcp` |
| Transport | Streamable HTTP (stateless, JSON responses) |
| Sign-in | OAuth 2.1 with PKCE and dynamic client registration. You sign in with your Skaler account. |
| Tools | 18: 14 read, 4 write |
| Official MCP Registry | `app.skaler/skaler` |
| Full docs | https://skaler.app/mcp |

## Connect

**Claude (claude.ai and desktop)**

1. Open Claude → Settings → Connectors.
2. Click "Add custom connector", name it Skaler and paste the server URL.
3. Click Connect, sign in to Skaler and allow access.

**ChatGPT (Plus / Pro / Business)**

1. Open ChatGPT → Settings → Apps & Connectors → Advanced, and turn on Developer mode.
2. Back in Apps & Connectors, click Create, name it Skaler and paste the server URL.
3. Choose OAuth, click Create, then sign in to Skaler and allow access.

**Claude Code**

```bash
claude mcp add --transport http skaler https://skaler.app/api/mcp
```

Then run `/mcp` in Claude Code, pick `skaler` and sign in.

**Codex**

```bash
codex mcp add skaler --url https://skaler.app/api/mcp
codex mcp login skaler
```

**Cursor and other MCP clients**

```json
{
  "mcpServers": {
    "skaler": {
      "url": "https://skaler.app/api/mcp"
    }
  }
}
```

## Who can use it

Any Skaler account. The demo ad account is free, so you can try every tool without a plan. Your own ad accounts need a Skaler plan (from $83/mo billed yearly).

## Tools

### Your ad account

Read-only analytics for the Meta ad accounts you connected in Skaler, plus a demo account.

| Tool | Effect | What it does |
|---|---|---|
| `get_workspace` | Read | Your plan, ad accounts (always including the demo account), brands and AI-tag categories. Assistants call it first. |
| `query_creatives` | Read | Ranked ads with spend, ROAS, CPA, CTR, hook and hold rate, Skaler's four creative scores, AI tags and thumbnails. Filter and group by any of them. |
| `get_ai_tag_performance` | Read | Which hooks, angles, personas, formats and offers win in the account, with spend share for each. |
| `get_account_summary` | Read | KPIs against the previous period, a daily trend and the top ads. Can return a ready-made Slack or markdown update. |
| `get_breakdowns` | Read | Spend or ROAS split by platform, placement, format, country, age and gender. |
| `get_ad_insights` | Read | The transcript and AI analysis already created in Skaler for one ad. |
| `list_reports` | Read | Your saved Skaler reports (sample reports on the demo account). |
| `get_report` | Read | Runs one of your saved reports with live data. |

### Competitor research

Read-only lookups in Skaler's library of public Meta ads. No ad account needed.

| Tool | Effect | What it does |
|---|---|---|
| `search_winning_ads` | Read | Finds winning ads in any niche from Skaler's ad library, with run time and scaling signals. |
| `spy_on_brand` | Read | What one brand is running and scaling right now: active ads, formats, landing pages and longest runners. |
| `analyze_ad_creative` | Read | Breaks down one ad: hook, angle, structure, copy and why it likely works. |
| `list_tracked_brands` | Read | The brands you follow in Skaler's Brand Spy. |

### Launching

These create things in your Meta ad account. Your assistant asks before each call, and everything starts paused unless you say otherwise.

| Tool | Effect | What it does |
|---|---|---|
| `get_launch_options` | Read | Your campaigns (CBO or ABO), ad sets, Pages, Instagram accounts and call-to-action types. |
| `create_campaign` | Write to Meta (paused by default) | A new, empty Meta campaign with any objective, ABO or CBO. |
| `create_launch_draft` | Write (Skaler draft only, nothing reaches Meta) | Builds a draft in Skaler's launcher from creative links: which ad set to copy, copy, link, budget and schedule. Returns a summary and a link to review it. |
| `launch_draft` | Write to Meta (spends money only if you ask for active or a scheduled start) | Uploads the creatives and creates the ad sets and ads from an approved draft. Needs the ad count and ad set name from the draft summary. |
| `get_launch_status` | Read | What a launch created, with Meta ad and ad set IDs, and why any ad failed. |

### Feedback

| Tool | Effect | What it does |
|---|---|---|
| `send_feedback` | Write (a message to Skaler support) | Sends a bug report or idea to the Skaler team when you ask your assistant to. |

## Limits

- 500 tool calls per user in any 24 hours.
- Up to 31 days per question; 7 to 14 days answer fastest.
- `create_campaign` 20 a day, `create_launch_draft` 50 a day, `launch_draft` 20 a day, `send_feedback` 10 a day.
- Monthly plans include MCP calls. Credit-based plans use 1 credit per data or competitor lookup.

## Safety

- Analytics and competitor tools only read. They can't change anything in Meta.
- Campaigns, ad sets and ads are created paused unless you ask for active or give a start time.
- A draft launches only after you've seen its summary.
- It never pauses, edits or deletes existing campaigns, ad sets or ads.
- It never sees your Meta login or access tokens, and only reaches ad accounts you connected in Skaler.

Disconnect any time in Skaler under Account Settings → Connected AI apps.

## Example prompts

- "Which of my ads had the best ROAS in the last 14 days, and which hooks do they share?"
- "Write a Slack update on last week's account performance."
- "What is this competitor scaling right now?"
- "Build a launch draft from these three creative links in my testing campaign. Keep it paused."

## Support

Questions and bug reports: support@skaler.app, or ask your assistant to use `send_feedback`.

## Files

- `server.json`: the entry published to the official MCP Registry.
- `lhm.plugin.json`: manifest for the LobeHub MCP marketplace.

## License

The documentation and configuration files in this repository may be copied freely. The Skaler service and MCP server are proprietary; use of the service is covered by the [Skaler terms of service](https://skaler.app/terms-of-service).
