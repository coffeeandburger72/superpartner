# WeChat Chat Parser

This file teaches Claude how to parse WeChat chat exports. When a WeChat export is provided, follow all instructions below to extract messages, identify senders, and note media references.

---

## Input Detection

### If input is a CSV file

CSV exports are produced by tools such as WeChatExporter and WechatExport. Read the header row first to auto-detect columns before processing any data rows.

**Common column patterns:**

| Tool / Locale | Typical header |
|---|---|
| English export tools | `Date,Sender,Type,Content` |
| Chinese export tools | `时间,发送者,类型,内容` |

After reading the header row, map the detected column names to the canonical fields:

- **timestamp** — `Date` or `时间` (or any column whose name contains "date", "time", "时间")
- **sender** — `Sender` or `发送者` (or any column whose name contains "sender", "from", "发送")
- **type** — `Type` or `类型` (or any column whose name contains "type", "类型")
- **content** — `Content` or `内容` (or any column whose name contains "content", "message", "内容")

If any required column cannot be mapped, stop and ask the user to confirm the column names.

**Handling quoted fields:** Content fields may contain commas. Always parse CSV with proper quote handling — a field wrapped in double quotes is a single value even if it contains commas or newlines.

### If input is a plain text file

Plain text exports come from manual copy-paste out of WeChat or from certain export utilities. See the Plain Text Parsing Rules section below.

---

## CSV Parsing Rules

### Type field — for v1, only process Text rows

The `Type` (or `类型`) field indicates message content type. Recognized values:

| Value | Meaning |
|---|---|
| `Text` / `文本` | Plain text message — **process this** |
| `Image` / `图片` | Photo — count only, skip content |
| `Sticker` / `表情` | Sticker / emoji sticker — count only, skip content |
| `Voice` / `语音` | Voice message — count only, skip content |
| `Video` / `视频` | Video — count only, skip content |
| `System` / `系统` | System event — skip entirely, do not count |

For any unrecognized Type value: skip the row and do not count it.

### Timestamp parsing

WeChat CSV timestamps commonly appear in these formats:

- `2026-03-15 14:32:01`
- `2026/03/15 14:32:01`
- `2026-03-15T14:32:01`

Parse whichever format is present. Store the timestamp alongside each message.

### Multi-line content in CSV

Some export tools wrap multi-line content inside a quoted field with literal newlines. Treat the entire quoted value as a single message body.

---

## Plain Text Parsing Rules

### Date header lines

Lines that consist solely of a date string mark the boundary between days. Detect and record the current date, but do not treat them as messages. Supported formats:

- `2026-03-15`
- `2026/03/15`
- `2026年3月15日`
- `2026年03月15日`

### Message lines

After a date header, each message appears in one of two layouts:

**Layout A — sender on its own line:**
```
Sender Name
Message content here
```

**Layout B — sender and content on the same line:**
```
Sender Name: Message content here
```

For Layout A: the sender name is on a line by itself (no colon), and the very next line(s) are the message body. Continuation lines (those that do not start a new sender name and are not a date header or system line) belong to the same message body.

For Layout B: split on the first `:` to get sender name and message body.

### System messages — skip entirely

Do not store or count lines that match any of the following:

- Lines starting with `—` or `–` (em-dash or en-dash)
- Lines containing `recalled a message`
- Lines containing `撤回了一条消息`
- Lines containing `你撤回了一条消息`
- Lines that are standalone date headers (handled separately above)
- Empty lines

---

## Sender Identification

### From CSV exports

Read all distinct values in the `Sender` / `发送者` column. The two most frequent non-empty values are the two main senders.

### From plain text exports

Collect all distinct sender names encountered while parsing. The two most frequent are the two main senders.

### Mapping senders

After identifying the two senders:

- The **"user"** is typically the person who exported the chat. If the export tool includes a self-label (e.g., a consistent name like "Me" or the device owner's WeChat name), use that.
- The **"partner"** is the other sender.

If more than 2 distinct senders are found, warn the user:

> "This appears to be a group chat with multiple senders: [list names]. Which sender is your partner?"

Wait for the user to specify before proceeding.

---

## No Media Support

WeChat exports produced by third-party tools rarely bundle the original image or audio files alongside the export. As a result:

- If CSV rows show `Image`, `Sticker`, `Voice`, or `Video` types, count each type separately but do not attempt to process or reference file paths.
- If plain text contains lines that appear to be media placeholders (e.g., `[图片]`, `[语音]`, `[视频]`, `[贴图]`, `[Photo]`, `[Sticker]`, `[Voice]`), count them but skip processing.
- Do not attempt to locate or read any media files. Media analysis is out of scope for v1.

---

## Output Summary

After parsing is complete, display a summary to the user:

```
Platform confirmed: WeChat
Date range: <first message date> → <last message date>
Messages:
  <Sender 1 name>: <count>
  <Sender 2 name>: <count>
Media (not processed):
  Images: <X>
  Stickers: <Y>
  Voice messages: <Z>
  Videos: <W>
```

Then pass the structured data (messages list, sender map) to the next stage of the skill.
