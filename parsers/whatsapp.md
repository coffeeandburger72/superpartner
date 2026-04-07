# WhatsApp Chat Parser

This file teaches Claude how to parse WhatsApp chat exports. When a WhatsApp export is provided, follow all instructions below to extract messages, identify senders, and collect media references.

---

## Input Detection

### If input is a `.zip` file

Run the following via Bash to unzip the export:

```bash
unzip -o <file> -d /tmp/superpartner-import-<slug>/
```

Where `<slug>` is a short identifier derived from the filename (e.g., `jane-doe`).

After unzipping:
- Locate `_chat.txt` — it may be at the root of the extracted directory or inside a subdirectory. Search recursively if needed.
- Glob for media files: `*.jpg`, `*.png`, `*.webp` — collect their full absolute paths.

### If input is a `.txt` file

Use the file directly as the chat transcript. No unzipping required.

---

## Text Parsing Rules

### Supported timestamp formats

Parse lines that begin with any of the following date/time patterns as the start of a new message:

- **Primary format:** `[DD/MM/YYYY, HH:MM:SS] Sender Name: message body`
- **Alternative format (some locales):** `DD/MM/YYYY, HH:MM - Sender Name: message body`
- **Alternative format (US locale):** `[M/D/YY, H:MM:SS AM/PM] Sender Name: message body`

Extract: timestamp, sender name, and message body.

### Multi-line messages

Lines that do not begin with a recognized date/time prefix belong to the previous message. Append them to that message's body (preserve newlines).

### System messages — skip entirely

Do not count the following as real messages. Detect and skip lines matching these patterns:

- `Messages and calls are end-to-end encrypted`
- `You deleted this message`
- `This message was deleted`
- `This message was edited` (as a standalone system line)
- `created group`
- `added` (e.g., "X added Y")
- `left` (e.g., "X left")
- Any line where, after stripping the timestamp, there is no `SenderName:` colon structure — treat as a system event and skip.

### Media references

Lines containing `<attached: FILENAME>` indicate a media file. Extract `FILENAME` and resolve it to the full absolute path within the unzipped directory (if the input was a `.zip`). Store this alongside the message.

### Edited message suffix

Strip the Unicode-prefixed suffix `‎<This message was edited>` from any message body. Keep all content before it.

### View-once messages

Lines matching `You received a view once message` should be recorded with content `[view-once]`. No further content is available.

### Deleted messages

Lines matching `You deleted this message` or `This message was deleted` — skip entirely. Do not store these.

---

## Media Handling

After collecting all media file paths from the export:

### Photos — `.jpg`, `.png`

Collect all photo file paths into a list. These paths will be passed to analyzers later.

### Stickers — `.webp`

Collect all `.webp` file paths. If there are more than 20 stickers total, sample evenly:

- Divide total count by 20 to get step size (e.g., 80 stickers → step = 4, pick every 4th one)
- Store the sampled paths list (up to 20 stickers)

### Ignored formats

Do not collect or process: `.mp4`, `.opus`, `.m4a`, `.mp3` — audio and video are out of scope for v1.

---

## Sender Identification

After parsing the first 20 messages, identify the two main senders:

- The **"user"** (the person who exported the chat) can be detected via system messages like `You deleted this message` — the "You" refers to the exporter. The exporter's name is typically one of the two most frequent senders.
- The **"partner"** is the other sender.

If more than 2 distinct senders are detected (indicating a group chat), warn the user:

> "This appears to be a group chat with multiple senders: [list names]. Which sender is your partner?"

Wait for the user to specify before proceeding.

---

## Output Summary

After parsing is complete, display a summary to the user:

```
Platform confirmed: WhatsApp
Date range: <first message date> → <last message date>
Messages:
  <Sender 1 name>: <count>
  <Sender 2 name>: <count>
Media:
  Photos: <X>
  Stickers: <Y total> (<Z sampled>)
```

Then pass the structured data (messages list, sender map, media paths) to the next stage of the skill.
