# ai-skills

A collection of AI agent skills I build and use daily — for [Hermes Agent](https://github.com/NousResearch/hermes-agent) and Claude Code.

Every skill here runs in my actual workflow. No demos, no toys — if it's in this repo, it's doing real work for me every day.

## Skills

| Skill | Agent | What it does |
|-------|-------|--------------|
| [daily-ai-tech-discord-briefing](hermes/daily-ai-tech-discord-briefing/) | Hermes | Sends a daily AI/tech briefing (news + trending GitHub projects) to a Discord channel at 6 AM, every morning, fully unattended |

## daily-ai-tech-discord-briefing

I wake up to this every morning at 6 AM in my `#tech-research` Discord channel:

<p>
  <img src="hermes/daily-ai-tech-discord-briefing/assets/discord-briefing-1.png" width="300" alt="Daily briefing — AI/tech highlights" />
  <img src="hermes/daily-ai-tech-discord-briefing/assets/discord-briefing-2.png" width="300" alt="Daily briefing — hot GitHub projects" />
</p>

The briefing covers, in a fixed scannable format:

1. **Today's AI/Tech Highlights** — cross-checked from Techmeme, Google News, and official sources
2. **Hot GitHub Projects TOP 5** — filtered from GitHub Trending toward AI agents, dev tooling, and infra
3. **Practical Takeaways** — what it means for builders
4. **One-Line Opinion**

### Install

```bash
mkdir -p ~/.hermes/skills/research/daily-ai-tech-discord-briefing
curl -o ~/.hermes/skills/research/daily-ai-tech-discord-briefing/SKILL.md \
  https://raw.githubusercontent.com/loydkim/ai-skills/main/hermes/daily-ai-tech-discord-briefing/SKILL.md
```

Then in a new Hermes session:

```text
Load the daily-ai-tech-discord-briefing skill and create a daily 6 AM cron job
that sends the briefing to my Discord #tech-research channel.
```

Full setup guide, cron configuration, troubleshooting, and common pitfalls: [SKILL.md](hermes/daily-ai-tech-discord-briefing/SKILL.md)

## Repo structure

```
ai-skills/
├── hermes/          # Skills for Hermes Agent (NousResearch/hermes-agent)
│   └── daily-ai-tech-discord-briefing/
│       ├── SKILL.md
│       └── assets/
└── claude-code/     # Skills for Claude Code (coming soon)
```

## License

MIT — see [LICENSE](LICENSE). Use them, fork them, make them yours.
