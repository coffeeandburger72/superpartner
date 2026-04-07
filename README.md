<div align="center">

# superpartner

**I finally got her a gift that made her smile.**

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Drop a chat export. Get a living profile of your partner — personality, interests, important dates, gift ideas, dating plans, and relationship insights. All evidence-based, all from your real conversations.

[Install](#install) | [How It Works](#how-it-works) | [Use Cases](#use-cases) | [Commands](#commands) | [Privacy](#privacy) | [**繁體中文**](README_ZH.md)

</div>

---

You know that feeling — her birthday is next week and you're blanking on gift ideas. Or you want to plan a date but can't remember if she mentioned wanting to try that new restaurant or the pottery class. Or you're in a long-distance relationship and running out of creative ways to stay connected.

**superpartner reads your chat history and builds a profile that actually knows her.** Not generic "buy flowers" advice — real suggestions grounded in what she's said, what she's shown interest in, and what makes her light up.

---

## Install

**Requirements:** [Claude Code](https://docs.anthropic.com/en/docs/claude-code)

```bash
# Install globally (available in all projects)
git clone https://github.com/coffeeandburger72/superpartner.git ~/.claude/skills/superpartner
```

That's it. One command.

---

## How It Works

```
You:     /superpartner
Claude:  What's your partner's name or nickname?
You:     Sarah
Claude:  How long have you been together, and what stage?
You:     Dating 6 months, long-distance
Claude:  Drop your chat export file.
You:     ~/Downloads/WhatsApp Chat - Sarah.zip

Claude:  Found 3,241 messages from Jan to Jul between you and Sarah on WhatsApp.
         Proceeding with analysis.

         ... analyzing personality across 6 dimensions ...
         ... mapping interests from messages and photos ...
         ... extracting dates, milestones, and upcoming events ...
         ... building relationship dynamics profile ...

         Done! Here's what I found:

         Personality: Warm and nurturing, plans ahead, expressive through
         stickers and voice notes, night owl who opens up after 9pm

         Top interests: Hiking, Japanese food, studio pottery, indie films

         Next occasion: Your 6-month anniversary (Jul 15)

         Top gift idea: A pottery workshop voucher for two — she's mentioned
         wanting to try wheel-throwing 3 times in the last month

         Use /superpartner gifts sarah, /superpartner dates sarah,
         or /superpartner ask sarah <question> anytime.
```

---

## Use Cases

### "Her birthday is in 2 weeks and I have no idea what to get"

```
/superpartner gifts sarah
```

Regenerates personalized gift ideas based on her interests, wishlist hints from chat, and her love language. Not generic — every suggestion is backed by something she actually said or showed interest in.

---

### "I want to plan something special this weekend"

```
/superpartner dates sarah
```

Generates date ideas tailored to what she enjoys, your relationship stage, and your location. Long-distance? It adapts — virtual movie nights, care packages, countdown activities for your next visit.

---

### "She seemed off lately and I'm not sure why"

```
/superpartner guide sarah
```

Your relationship guide — communication patterns, conflict resolution style, emotional needs, and how to navigate difficult conversations based on how she actually communicates. No textbook advice, just patterns from your real dynamic.

---

### "Does she actually like sushi or am I imagining that?"

```
/superpartner ask sarah does she like sushi?
```

Ask anything about your partner. Answers grounded in profile data with evidence — exact quotes, photo references, and behavioral patterns from your conversations.

---

### "We just had a new trip together and I want to update her profile"

```
/superpartner update sarah
```

Drop a new chat export. The profile merges intelligently — new data enriches existing insights without overwriting. Gift ideas and date plans auto-regenerate with fresh context.

---

### "I don't want to forget her mom's birthday again"

After building a profile, superpartner offers to set up reminders for upcoming occasions — birthdays, anniversaries, trips, events. You get a nudge 2 weeks out and 3 days before, with your top gift pick ready.

---

## Commands

| Command | What it does |
|---------|-------------|
| `/superpartner` | Create a new partner profile from chat export |
| `/superpartner list` | See all profiles at a glance |
| `/superpartner update <name>` | Add new chat data to an existing profile |
| `/superpartner gifts <name>` | Regenerate gift ideas |
| `/superpartner dates <name>` | Regenerate dating plans |
| `/superpartner guide <name>` | Regenerate relationship guide |
| `/superpartner ask <name> <question>` | Ask anything about your partner |
| `/superpartner delete <name>` | Remove a profile completely |

---

## What Gets Generated

For each partner, superpartner creates 6 profile files:

| File | Contents |
|------|----------|
| **persona.md** | Personality across 6 layers — communication style, emotional patterns, values, attachment orientation, with quoted evidence |
| **interests.md** | Categorized interests, food preferences, hobbies, wishlist items — all tagged with how they were detected |
| **occasions.md** | Birthdays, anniversaries, upcoming events, a 90-day gift calendar with prep timelines |
| **gifts.md** | Personalized gift ideas ranked by fit, calibrated to love language and relationship stage |
| **dating_ideas.md** | Date plans organized by type, with LDR adaptations if applicable |
| **relationship_guide.md** | Communication playbook — how to handle tough conversations, emotional needs, what to watch for |

Every finding is evidence-tagged: `(explicit)` from direct statements, `(inferred)` from behavioral patterns, or `(photo)` from image analysis. Profiles are written in the language of your chats.

---

## Supported Platforms

| Platform | Export Format | How to Export |
|----------|-------------|---------------|
| **WhatsApp** | `.zip` | Chat > More > Export Chat (with media) |
| **WeChat** | `.csv` or text | Use third-party export tools |
| **LINE** | `.txt` | Settings > Chats > Export Chat History |
| **Any platform** | Pasted text | Copy-paste conversation directly |

---

## Privacy

Your data stays on your machine. Always.

- All partner profiles are stored locally in `partners/` and gitignored by default
- Chat exports are never committed, uploaded, or sent anywhere
- No telemetry, no analytics, no cloud sync
- `delete` command fully removes all traces — profile files, scheduled reminders, and memory entries

---

## License

MIT. Free forever.
