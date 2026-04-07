# Merger — Incremental Update Instructions

You are handling an `update` subcommand. New chat data has been provided for an existing partner profile. Your job is to merge the new findings into the existing profile carefully, surgically, and without losing any existing information.

---

## Pre-Merge Setup

1. **Read the existing profile files:**
   - `partners/<slug>/meta.json` — version, sources, timestamps, stats
   - `partners/<slug>/persona.md` — personality, communication style, values
   - `partners/<slug>/interests.md` — hobbies, food, activities, topics
   - `partners/<slug>/occasions.md` — birthdays, anniversaries, events

2. **Parse the new chat data** using the same parser selected by SKILL.md for the `update` subcommand (WhatsApp, Telegram, iMessage, etc.). Do not re-select the parser yourself — SKILL.md has already determined it.

3. **Note the date range** of the new data and compare it against the `sources` array in `meta.json`. Identify whether the new data is:
   - A continuation (later messages after the last known date)
   - An overlap (some messages already covered)
   - A gap-fill (messages from a date range not previously included)

   Log this observation before proceeding.

---

## Analysis Phase

Run each analyzer on the **new data only**. Do not re-analyze the full history. Produce a list of findings as if you were building the profile fresh from just the new data.

Analyzers to run (same as create flow):
- `analyzers/persona_analyzer.md`
- `analyzers/interests_analyzer.md`
- `analyzers/occasions_analyzer.md`

After collecting findings from the new data, compare each finding against the content already in the existing profile files.

---

## Merge Classification

For **each finding** from the new analysis, classify it into one of the four categories below and take the appropriate action.

---

### 1. New Information

**Definition:** This finding is not present anywhere in the existing profile.

**Action:**
- Append the new item to the appropriate section in the appropriate file.
- Use the **Edit tool** to make a surgical insertion — do NOT use the Write tool (which would overwrite the entire file).
- Tag the addition with the current date: `(added: YYYY-MM-DD)`
- Place it in the correct section. For example:
  - A new cuisine she mentioned → Edit `interests.md`, append to the "Food & Drinks" section
  - A new personality observation → Edit `persona.md`, append to the relevant trait section
  - A newly mentioned upcoming event → Edit `occasions.md`, add to the appropriate section

**Example edit result:**
```
- Loves matcha lattes (added: 2025-11-03)
```

---

### 2. Confirming Information

**Definition:** This finding is already in the profile and the new data simply reinforces it.

**Action:**
- Skip — do not duplicate the entry.
- **Optional confidence upgrade:** If the existing entry has a `(weak)` confidence tag and the new data provides additional evidence, upgrade it:
  - `(weak)` → `(moderate)` if new data supports it once more
  - `(moderate)` → `(strong)` if new data supports it repeatedly
  - Use the Edit tool to update only the confidence tag in place.

---

### 3. Contradicting Information

**Definition:** The new data conflicts with something already in the profile.

**Action:**
- Do **not** make any change automatically.
- Present the conflict to the user in this exact format:

  > **Conflict in [file] > [section]:** Existing: '[old info]' vs New: '[new info]'. Which should I keep? (old / new / both)

- **Wait for the user's response** before editing anything.
- Once the user responds:
  - `old` → leave the file unchanged
  - `new` → use Edit tool to replace the old entry with the new one, tagged `(updated: YYYY-MM-DD)`
  - `both` → use Edit tool to keep the old entry and append the new one with the note `(alternative view, added: YYYY-MM-DD)`

---

### 4. Outdated Information

**Definition:** The new data strongly implies that an existing entry is no longer true. Examples: she changed jobs, moved cities, ended a friendship, changed a preference she used to hold firmly.

**Action:**
- Do **not** silently overwrite.
- Flag the potential outdated entry to the user in this exact format:

  > **Update in [file] > [section]:** '[old info]' appears outdated based on new message: '[new quote]'. Replace? (yes / no)

- **Wait for the user's response** before editing.
- Once the user responds:
  - `yes` → use Edit tool to replace old entry, tagged `(updated: YYYY-MM-DD)`
  - `no` → leave the file unchanged

---

## Post-Merge Steps

After all findings are classified and all user-prompted conflicts/updates are resolved, perform the following:

### 1. Update `meta.json`

Use the Edit tool to make the following changes to `partners/<slug>/meta.json`:

- Set `updated_at` to the current ISO 8601 timestamp (e.g. `"2025-11-03T14:32:00Z"`)
- Append a new entry to the `sources` array describing the new data:
  ```json
  {
    "file": "<original filename>",
    "parser": "<parser used>",
    "date_range": { "from": "YYYY-MM-DD", "to": "YYYY-MM-DD" },
    "added_at": "YYYY-MM-DD"
  }
  ```
- Increment the version field: `"v1"` → `"v2"`, `"v2"` → `"v3"`, etc.
- If new photos or stickers were analyzed, update the relevant fields in the `stats` object.

### 2. Auto-regenerate `gifts.md`

- Read `builders/gifts_builder.md`
- Using the now-updated `interests.md`, `persona.md`, and `occasions.md` as input, regenerate `partners/<slug>/gifts.md` in full.
- Use the Write tool for this file since it is a derived file and full regeneration is correct.

### 3. Auto-regenerate `dating_ideas.md`

- Read `builders/dating_ideas_builder.md`
- Using the now-updated profile files as input, regenerate `partners/<slug>/dating_ideas.md` in full.
- Use the Write tool for this file since it is a derived file and full regeneration is correct.

### 4. Auto-regenerate `relationship_guide.md`

- Read `builders/relationship_guide_builder.md`
- Using the now-updated `persona.md` and all available profile context, regenerate `partners/<slug>/relationship_guide.md` in full.
- Use the Write tool for this file since it is a derived file and full regeneration is correct.

### 5. Display Merge Summary

After all steps are complete, print a summary to the user:

```
Merge complete for <partner name> (v<old> → v<new>):

  X new items added
    - interests.md: <sections touched>
    - persona.md: <sections touched>
    - occasions.md: <sections touched>

  Y conflicts presented to user
    - Z resolved as "keep old"
    - Z resolved as "use new"
    - Z resolved as "keep both"

  W items confirmed (already in profile, no change needed)

  V outdated items flagged
    - Z replaced
    - Z kept as-is

  gifts.md regenerated
  dating_ideas.md regenerated
  relationship_guide.md regenerated
  meta.json updated → v<new>, sources: <N> total
```

---

## Critical Rules

- **NEVER silently overwrite existing data.** Every deletion or replacement must be approved by the user first.
- **Use the Edit tool for surgical updates** to persona.md, interests.md, occasions.md, and meta.json. Never use Write for these files.
- **Use Write only for derived files** (gifts.md, dating_ideas.md, relationship_guide.md) since they are fully regenerated.
- **Preserve ALL existing evidence and quotes.** Appending is safe; removing requires explicit user approval.
- **Date-tag every addition** with `(added: YYYY-MM-DD)` so the user can see how the profile evolved over time.
- **Date-tag every update** with `(updated: YYYY-MM-DD)` when replacing an outdated or conflicting entry.
- **Always regenerate gifts.md, dating_ideas.md, and relationship_guide.md** after the merge — they are derived files and must reflect the latest profile state.
- **Never skip the conflict/outdated prompts** even if the contradiction seems minor. The user decides, not you.
