# Superpartner

Claude Code skill that builds partner profiles from chat exports (WhatsApp/WeChat/LINE).

## Structure

- `SKILL.md` — Router/orchestrator, invoked via `/superpartner`
- `parsers/*.md` — Platform-specific chat parsers (whatsapp, wechat, line)
- `analyzers/*.md` — 4 analyzers (persona [6 layers], interests, occasions, relationship [7 dimensions])
- `builders/*.md` — 6 builders (persona, interests, occasions, gifts, dating_ideas, relationship_guide)
- `merger.md` — Incremental update logic
- `sherlock.md` — Interactive behavioral analysis session (sherlock subcommand)
- `partners/<slug>/` — Generated profile output (persona.md, interests.md, occasions.md, gifts.md, dating_ideas.md, relationship_guide.md, meta.json)
- `examples/sarah/` — Fictional demo profile (public, committed to git) — all 7 profile files in English
- `docs/images/` — Banner PNGs and feature card PNGs for README (EN + 繁中)
- `docs/posters/` — HTML poster mockups with shared CSS (`poster-common.css`); tracked in git for community contributions
- `.claude-plugin/` — Plugin marketplace manifest (`plugin.json`) and catalog (`marketplace.json`)
- `.claude/settings.json` — Project-level Claude Code settings (committed); includes SessionStart hook for auto-update

## Privacy

- `partners/` data is gitignored — never commit personal profile data or chat exports
- Example names in prompt files must be generic (e.g., `jane-doe`) — never use real partner names
- Git author is anonymized — do not reconfigure to real identity
- NEVER include Co-Authored-By lines in git commits

## Key Patterns

- All prompt files are `.md` — this is a prompt-only project, no executable code
- Analyzers hold results in context; builders write files via Write tool
- Every finding must be evidence-tagged: `(explicit)`, `(inferred)`, or `(photo)` with original-language quotes
- Profile files written in detected chat language (e.g., Cantonese); section headers stay English
- `README.md` (English) and `README_ZH.md` (繁體中文, 書面語 not Cantonese) — keep examples consistent across both
- Chinese README uses 莎莎 as the example partner name; English uses Sarah
- Write tool requires reading a file first before overwriting — always Read (even 5 lines) before Write on existing files
- Primary install: `/plugin marketplace add coffeeandburger72/superpartner` then `/plugin install superpartner@superpartner-marketplace`
- Legacy install: symlink `~/.claude/skills/superpartner` → this directory
- Plugin uses `"skills": "./"` in plugin.json — SKILL.md at repo root with `name: superpartner` frontmatter drives discovery
- Feature card PNGs exported at 2x (2400x600) with transparent background via puppeteer (`omitBackground: true`, body bg set to transparent)
- Poster CTA pill pattern: `<span class="slash">/</span><span class="cmd">superpartner gifts </span><span class="arg">sarah</span>` — subcommand in cmd span with trailing space, name in arg span
- Mode chips (`.mode-chip`) use filled gradient bg; skill chips (`.skill-chip`) use outlined gray border — visually distinct
- Relationship science frameworks (attachment theory, love languages, etc.) are implicit — never reference book titles or author names in any prompt output
- Love language abbreviations: QT, WA, RG, AS, PT — consistent across gifts and dating builders
- Attachment terms: Secure / Anxious / Avoidant / Mixed — behavioral descriptions only, no clinical labels
- LDR flag (`ldr: true/false`) flows from SKILL.md intake through all analyzers and builders
- Web search for concrete picks is always opt-in — never auto-triggered
- Sherlock sessions are interactive and stateless — no files written, no profiles modified, no cron jobs created
- Proactive behaviors (cron reminders, post-update nudge) require explicit user consent
- Platform icons use official brand SVG paths from simple-icons; badge colors: WhatsApp `#25D366`, WeChat `#07C160`, LINE `#06C755`
- README badges use `style=for-the-badge` (shields.io) to match the visual weight of poster skill-chip pills
- Hero banner (1200x340) shows icon-only platform circles (no text labels) + compact privacy pill; social banner (1280x640) shows full badge row with text

## Gotchas

- Markdown syntax (blockquotes, bold, etc.) does NOT render inside `<div>` HTML blocks on GitHub — use HTML tags (`<em>`, `<strong>`, `<blockquote>`) instead
- `.claude/` is gitignored but `.claude/settings.json` is force-tracked (`git add -f`) — negation rules don't work when parent dir is ignored
- WhatsApp exports vary by locale — US format `[M/D/YYYY, H:MM:SS AM/PM]` vs UK `[DD/MM/YYYY, HH:MM:SS]`
- Chat file can be 2800+ lines — read in chunks (150-200 lines) to avoid token limits
- Stickers (.webp) must be sampled (~20 max) — don't try to read all 100+
- Relative date inference matters: "提早少少你可能會miss" means the date is weeks away, not months
- Always confirm date interpretations with user — analyzers can misjudge month from context
- `git-filter-repo` available for history sanitization — use `--replace-text` for content, `--mailmap` for author
- Persona analyzer has 6 layers (0-5), relationship analyzer has 7 dimensions — keep count references consistent when adding new ones
- GitHub strips `<script>` and some CSS from SVGs in READMEs — banners must be static SVG only; use HTML poster for animated/interactive versions
- SVG `@import url()` for fonts may not load in GitHub — keep font fallbacks (system sans-serif)
- `.claude-plugin/` is NOT inside `.claude/` — it's a separate top-level dir, tracked normally (no `git add -f` needed)
- `docs/posters/` is tracked (not gitignored) — was removed from `.gitignore` for community contributions
- Marketplace `source` in marketplace.json must start with `./` not `.` — validator rejects bare `.`
- Puppeteer element screenshots capture bounding box — parent bg must also be transparent for rounded corners to show
