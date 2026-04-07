# LINE Chat Parser

This file teaches Claude how to parse LINE chat exports. When a LINE export is provided, follow all instructions below to extract messages, identify senders, and produce a structured summary.

---

## Input Detection

LINE exports are plain `.txt` files. There is no zip archive and no bundled media — LINE exports are text only.

Use the file directly as the chat transcript.

---

## Header Parsing

### First line — chat title

The first line identifies the chat type and the partner name:

- **Direct chat:** `[LINE] Chat with <Name>`
  - Extract `<Name>` as the partner name.
- **Group chat:** `[LINE] <Group Name>的聊天記錄`
  - Detect this as a group chat. Warn the user (see Sender Identification below).

### Second line — export timestamp

The second line is the export date:

```
Saved on: YYYY/MM/DD HH:MM
```

Parse and store this value for reference, but do not count it as a message.

---

## Text Parsing Rules

### Date headers

Lines of the form `YYYY/MM/DD（Day）` mark the start of a new calendar day. The day abbreviation is a CJK weekday character (e.g., `（六）` for Saturday, `（日）` for Sunday). Treat these as section dividers — do not count them as messages.

```
2026/03/15（六）
```

### Message lines

Each message occupies a single line with exactly three tab-separated fields:

```
HH:MM\tSender\tMessage content
```

- **Timestamp:** `HH:MM` (24-hour clock, no seconds)
- **Sender:** display name of the sender
- **Message content:** everything after the second tab

Parse the full date+time for each message by combining the most recent date header with the message's `HH:MM` timestamp.

### Multi-line messages

LINE does not wrap long messages across multiple lines in its export format. Each line is a self-contained message. Do not attempt to merge lines.

---

## Message Content Rules

### Stickers

If the message content field is exactly `[Sticker]`, record this message with content `[Sticker]` and increment the sticker count for that sender. These are used for frequency analysis.

### Photos

If the message content field is exactly `[Photo]`, record this message with content `[Photo]`. Note that no image file is available — LINE exports do not bundle media.

### Video and audio — skip

Lines where message content is `[Video]` or `[Audio]` — skip entirely. Do not store or count these.

### Unsent messages — skip

Lines where message content matches either of the following patterns — skip entirely:

- `[You unsent a message]`
- `[<Sender> unsent a message]` (any sender name in place of `<Sender>`)

### System messages — skip

Lines that have only **two** tab-separated fields (i.e., no sender field, or no message content field) are system events (e.g., join/leave notifications). Skip them entirely. Do not count them.

Example system message pattern to detect and skip:
```
HH:MM\tSome system event text
```

### URLs and links

URLs or links appearing within message content are kept as-is. Do not strip or replace them.

---

## Sender Identification

### Direct chat

The partner name is extracted from the first line: `[LINE] Chat with <Name>`.

After parsing the first 20 messages, identify the two senders:

- The sender whose name matches the extracted partner name is the **partner**.
- The other sender is the **user** (the person who exported the chat).

### Group chat

If the first line matches the group chat pattern (`的聊天記錄`), warn the user immediately before proceeding:

> "This appears to be a group chat. Multiple senders are present: [list names found in first 20 messages]. Which sender is your partner?"

Wait for the user to specify the partner before continuing.

---

## No Media Support

LINE exports contain text only. No image, sticker, video, or audio files are bundled with the export. Do not attempt to resolve or load any media paths. Record sticker and photo counts from tags in the text, but treat all media as unavailable.

---

## Output Summary

After parsing is complete, display a summary to the user:

```
Platform confirmed: LINE
Date range: <first message date> → <last message date>
Messages:
  <Sender 1 name>: <count>
  <Sender 2 name>: <count>
Media:
  Stickers: <X total> (no files available)
  Photos: <Y total> (no files available)
```

Then pass the structured data (messages list, sender map) to the next stage of the skill.
