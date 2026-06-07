---
name: daily-ai-tech-discord-briefing
description: Use when setting up a Hermes Agent cron job that sends a concise daily AI/tech briefing to a Discord tech-research channel, including current AI/tech news and hot GitHub AI/developer projects.
version: 1.0.0
author: Loyd Kim (@loydkim)
license: MIT
metadata:
  hermes:
    tags: [hermes, cron, discord, ai-news, tech-briefing, github-trending]
    related_skills: [hermes-agent]
---

# Daily AI/Tech Discord Briefing

## Overview

This skill helps a Hermes Agent user create a scheduled daily AI/tech briefing and automatically deliver it to a Discord channel such as `#tech-research`.

The briefing should be concise, practical, and written for busy technical readers. It focuses on:
- current AI/tech news
- hot GitHub AI/developer projects
- practical implications for builders, operators, product teams, and founders

It is designed for Hermes cron jobs. In a cron run, the final assistant response is automatically delivered to the configured Discord target. The cron task should not call `send_message` manually.

## When to Use

Use this skill when the user wants to:
- receive a daily AI/tech report in Discord
- send a scheduled report to `#tech-research` or a similar channel
- include a “Hot GitHub Projects TOP N” section
- create or maintain a Hermes cron job for recurring research briefings
- share a reusable setup with another Hermes Agent user

Do not use this for:
- one-off deep research reports that do not need scheduling
- non-Discord delivery unless you adapt the `deliver` target
- reports that require private paid data sources unless the user has configured those separately

## Prerequisites

The receiving user should have:

1. Hermes Agent installed and configured.
2. Discord gateway connected.
3. A Discord channel available, for example `#tech-research`.
4. Toolsets enabled for scheduled research runs:
   - `web`
   - `browser`
   - `terminal`
   - `skills`
5. A working model/provider for cron execution.

Useful checks:

```bash
hermes gateway status
hermes cron list
hermes tools list
```

If Discord delivery is uncertain, list available messaging targets from a Hermes session using the messaging/send-message tool, or check gateway configuration and logs.

## Recommended Output Shape

Use these sections in this exact order:

1. Today’s AI/Tech Highlights
2. Hot GitHub Projects TOP 5
3. Practical Takeaways
4. One-Line Opinion

Style:
- English by default
- concise and scannable
- practical rather than hype-heavy
- include GitHub links in the GitHub project section
- optionally include short source names for news items

If the user wants another language, adapt the section titles and tone while preserving the same structure.

## Source Workflow

### 1. Gather current AI/tech news

Use multiple live sources and cross-check claims.

Recommended source mix:
- Techmeme for high-signal tech conversation
- Google News / Google News RSS for recent AI/tech coverage
- official company blogs or release notes for verification
- Hacker News only as supplemental signal

Prefer a mix of:
- model or AI platform release
- developer workflow or coding-agent update
- infrastructure, semiconductor, or cloud item
- business/ecosystem movement
- one major platform/company strategy shift if relevant

Avoid:
- duplicate coverage of the same event
- thin opinion posts
- unverified PR-only claims
- forcing stories from blocked/paywalled sites when better sources exist

### 2. Gather hot GitHub projects

Primary source:

```text
https://github.com/trending
```

Collect:
- repository name
- GitHub URL
- one-line description
- total stars
- stars today, if visible

Prioritize:
- AI agents
- local AI tools
- model/tooling infrastructure
- developer productivity tools
- workflow automation
- multimodal or data tools with strong momentum

Skip unrelated viral repositories even if they are trending.

### 3. Synthesize for usefulness

For each news item, explain:
- what happened
- why it matters

For each GitHub repository, include:
- project name
- full GitHub URL
- short explanation of why it is hot/useful

## Cron Job Setup

### Option A: Create from Hermes CLI

A typical schedule for every morning at 6 AM:

```bash
hermes cron create '0 6 * * *'
```

When prompted, use a self-contained prompt like this:

```text
Prepare a concise daily AI/tech briefing in English.
Use live sources to gather current AI/tech news and hot GitHub AI/developer projects.

Output sections in this exact order:
1. Today’s AI/Tech Highlights
2. Hot GitHub Projects TOP 5
3. Practical Takeaways
4. One-Line Opinion

For the GitHub section, include each project's full GitHub URL.
Keep it concise, practical, and easy to scan.
The final response will be auto-delivered by the cron job; do not call send_message manually.
```

Set delivery to the Discord channel target, for example:

```text
discord:#tech-research
```

If your Hermes instance uses numeric Discord channel IDs, use the exact target returned by your messaging target list.

### Option B: Create from a Hermes session with the cron tool

Use a cron creation equivalent to:

```json
{
  "action": "create",
  "name": "daily-ai-tech-report",
  "schedule": "0 6 * * *",
  "deliver": "discord:#tech-research",
  "skills": ["daily-ai-tech-discord-briefing"],
  "enabled_toolsets": ["web", "browser", "terminal", "skills"],
  "prompt": "Prepare a concise daily AI/tech briefing in English. Use live sources to gather current AI/tech news and hot GitHub AI/developer projects. Output sections in this exact order: 1. Today’s AI/Tech Highlights 2. Hot GitHub Projects TOP 5 3. Practical Takeaways 4. One-Line Opinion. For the GitHub section, include each project's full GitHub URL. Keep it concise, practical, and easy to scan. The final response will be auto-delivered by the cron job; do not call send_message manually."
}
```

Notes:
- Omit `repeat` for a recurring cron schedule, or use the default recurring behavior for cron expressions.
- Cron runs start in a fresh session, so the prompt must be self-contained.
- Attach this skill to the cron job if it is installed on the receiving Hermes profile.
- Restrict enabled toolsets to reduce overhead and improve reliability.

## How Another User Can Install This Skill

If someone receives this `SKILL.md`, they can install it as a local Hermes skill by creating this folder structure:

```bash
mkdir -p ~/.hermes/skills/research/daily-ai-tech-discord-briefing
cp SKILL.md ~/.hermes/skills/research/daily-ai-tech-discord-briefing/SKILL.md
```

Then start a new Hermes session and ask:

```text
Load the daily-ai-tech-discord-briefing skill and create a daily 6 AM cron job that sends the briefing to my Discord #tech-research channel.
```

If they want Hermes to create the skill from the document directly, they can say:

```text
Create a Hermes skill from this SKILL.md content, then use it to set up a daily AI/tech Discord briefing cron job.
```

## Example Discord Post

This is an example of what the delivered Discord message should look like:

```text
Good morning — here’s the quick AI/tech brief for today.

1. Today’s AI/Tech Highlights
- Major model/platform update: A leading AI lab shipped or previewed a new capability that pushes agentic workflows closer to production use.
- Developer tooling: Coding agents and AI-assisted IDE workflows continue moving from demos into daily engineering pipelines.
- Infrastructure: Cloud and GPU providers are optimizing for faster inference and lower serving costs, which matters for AI apps at scale.
- Regulation/business: Enterprise AI adoption is increasingly shaped by data governance, security, and vendor lock-in concerns.

2. Hot GitHub Projects TOP 5
- owner/project-a — https://github.com/owner/project-a
  Useful because it addresses a practical agent, local AI, or developer workflow problem.
- owner/project-b — https://github.com/owner/project-b
  Trending because it has strong daily stars and clear utility for AI builders.
- owner/project-c — https://github.com/owner/project-c
  Interesting for teams experimenting with automation, evals, or model deployment.
- owner/project-d — https://github.com/owner/project-d
  Worth watching if you care about local-first AI or privacy-preserving workflows.
- owner/project-e — https://github.com/owner/project-e
  A practical tool with momentum in the developer community.

3. Practical Takeaways
- Track agent tooling, but evaluate it against real internal workflows, not demos.
- Watch inference cost and latency improvements; they can change product feasibility quickly.
- Keep GitHub Trending in the loop for early signals on developer behavior.

4. One-Line Opinion
The AI race is increasingly shifting from “who has the best model” to “who turns models into reliable workflows fastest.”
```

## Maintenance Workflow

When maintaining the scheduled job:

1. List jobs:

```bash
hermes cron list
```

2. Confirm:
- job is enabled
- schedule is correct
- delivery target points to the intended Discord channel
- last run status is `ok`
- there is no delivery error

3. If the wrong channel is used, update the `deliver` target.
4. If model/client errors happen before tool work starts, update the job to a known-good provider/model.
5. After changes, manually run the job once and verify both cron status and Discord delivery.

## Discord Troubleshooting

If the job runs but does not appear in Discord:

1. Check cron state:

```bash
hermes cron list
```

Look for:
- `last_status: ok`
- `last_delivery_error: null`

2. Check gateway status:

```bash
hermes gateway status
```

3. Check gateway logs:

```bash
tail -n 80 ~/.hermes/logs/gateway.log
```

4. Verify Discord bot permissions on the target channel:
- View Channels
- Send Messages
- Read Message History
- Use Slash Commands
- Embed Links, if rich links are desired

5. Check Discord channel filters in `~/.hermes/config.yaml`, especially:

```yaml
discord:
  require_mention: false
  free_response_channels: ''
  allowed_channels: ''
```

Important behavior:
- `allowed_channels` is a whitelist. If set to one channel ID, Hermes may ignore other channels.
- `free_response_channels` does not override `allowed_channels`.
- Gateway config changes usually require restart:

```bash
hermes gateway restart
```

## Common Pitfalls

1. Cron prompt is not self-contained.
   - Cron jobs run in fresh sessions. Include output format, language, tone, and delivery behavior in the prompt.

2. Calling `send_message` inside the cron run.
   - Do not do this. The cron scheduler auto-delivers the final response to `deliver`.

3. Using a human-readable channel name when Hermes expects a numeric Discord target.
   - List available messaging targets and use the exact target string.

4. GitHub section lacks links.
   - Always include full GitHub URLs for each project.

5. Trending repos are off-topic.
   - Filter toward AI, developer tooling, infra, and automation. Do not blindly take the top five overall.

6. News source gets blocked.
   - Do not waste time fighting anti-bot pages. Cross-check via Techmeme, Google News RSS, official blogs, or reputable secondary sources.

7. Too much detail.
   - The value is a fast morning briefing. Keep it concise.

## Verification Checklist

Before considering the setup complete:

- [ ] Discord gateway is connected.
- [ ] Target channel exists, for example `discord:#tech-research`.
- [ ] Cron job is enabled and scheduled for the desired time.
- [ ] Cron prompt is self-contained.
- [ ] Delivery target is configured on the cron job.
- [ ] Enabled toolsets include `web`, `browser`, `terminal`, and `skills`.
- [ ] A manual run completes with `last_status: ok`.
- [ ] The briefing appears in the Discord channel.
- [ ] The GitHub section includes full GitHub links.
- [ ] The output uses the four standard English sections.
