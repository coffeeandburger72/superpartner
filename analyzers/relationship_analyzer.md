# Relationship Analyzer

This file instructs Claude how to analyze the couple's dynamic from parsed chat history. When SKILL.md reads this file during the create flow, follow every instruction below to produce a structured relationship analysis.

---

## Input

You have the following data in context from the parser step:

- **Parsed messages** — a list of messages, each with: timestamp, sender name, message body
- **Sender map** — which sender is the "user" (exporter) and which is the "partner" (the person being profiled)
- **Date range** — the full span of messages available for analysis

**This analyzer examines both people equally.** Unlike persona and interests analysis (which focus on the partner), relationship analysis is about the dynamic between the two. Observe both senders' behavior, patterns, and contributions.

---

## Analysis Framework — 7 Dimensions

Work through each dimension in order. For every finding, cite evidence from the chat. Do not speculate beyond what the data supports.

---

### Dimension 1: Communication Patterns

Analyze the structural mechanics of how these two people communicate.

#### Initiation Ratio

Determine who starts conversations more often.

**How to count:**
- A new conversation starts when a message follows a gap of more than 4 hours with no prior messages
- Count the first message in each such session and record who sent it
- Compute: user-initiated sessions vs. partner-initiated sessions vs. total sessions
- Express as a ratio (e.g., "partner initiates 65% of conversations, user 35%")

**What to look for:**
- Is one person consistently the first to reach out each day?
- Are there stretches where the other person is always initiating (may indicate a context change, like one person traveling or being busier)?
- Does initiation pattern shift over time (early in the date range vs. later)?

#### Response Speed

Estimate the average response latency for each person.

**How to calculate:**
- Sample message exchanges where a message was sent by person A and the next message is from person B (no gap longer than 12 hours, to exclude sleep/offline gaps)
- Estimate quartiles: quick replies (under 5 min), normal replies (5-30 min), slow replies (30 min - 3 hours), very slow (3+ hours)
- Note: response speed often reflects context (work hours, sleeping) — flag any patterns

**What to look for:**
- Is one person consistently faster to reply?
- Does either person leave long gaps on certain topics (possible avoidance signal)?
- Do response times differ significantly between morning/afternoon/evening/night?

#### Peak Hours

Identify when the couple communicates most.

**How to measure:**
- Group messages by hour of day, then by time block:
  - Morning: 6am–12pm
  - Afternoon: 12pm–6pm
  - Evening: 6pm–10pm
  - Late night: 10pm–2am
  - Overnight: 2am–6am (flag if significant)
- Count messages per block across both senders combined
- Note if peak hours differ for each person

**What to look for:**
- Consistent patterns (e.g., always active during lunch, always chats before bed)
- "Good morning" and "good night" bookending — who sends first, who sends last
- Weekend vs. weekday shifts in active hours

#### Message Volume Balance

Assess whether the conversational load is shared equally.

**How to measure:**
- Total message count per sender
- Average message length (short 1-line vs. multi-sentence) per sender
- Who sends the last message before long gaps (possible sign of who "gives more" conversationally)

**What to look for:**
- Is one person carrying more of the conversational weight?
- Does imbalance correlate with context (e.g., one is always busier during work hours)?
- Has the balance shifted across the date range?

#### Conversation Starters

What topics does each person typically raise first when initiating?

**Categories to look for:**
- Daily life updates ("我食緊X", "今日好攰", "啱啱返到")
- Plans and logistics ("聽日做咩?", "我哋幾時去X")
- Sharing media or content (sending links, videos, memes, photos)
- Questions about the other person ("你食咗飯未?", "你而家點?")
- Emotional check-ins ("你ok嗎?", "點喇?")
- Complaints or venting
- Affectionate openers ("想你", "miss you")

Note which topic category each person defaults to when opening a conversation.

#### LDR-Specific Signals (activate when long-distance is detected or suspected)

If the relationship context indicates long-distance, or if chat patterns suggest it (different city/country references, "返嚟"/"過嚟" reunion language, timezone-mismatched message clusters), activate these additional scans:

- **Timezone gap detection:** Are messages from each person clustered at different hours? Does "good morning" from one person arrive during the other's afternoon?
- **Video/voice call references:** Count mentions of "打俾你", "video call", "facetime", "傾電話", "打畀你". Estimate frequency: daily, several times a week, weekly, less.
- **Message density around reunions:** Compare message volume in the week before a visit vs. normal baseline. Also check the week after — does engagement spike or dip post-reunion?
- **Reunion logistics volume:** How much conversation energy goes to planning visits (flights, accommodation, schedules) vs. emotional content?

**LDR auto-detection:** If not stated in intake but patterns suggest long-distance, flag in output:
> `LDR SIGNAL DETECTED: [evidence — e.g., "partner references 台北 while user references 香港", "reunion language appears 8+ times"]. Confirm with user before applying LDR-specific analysis.`

---

### Dimension 2: Shared vs. Individual Interests

Identify which topics energize both people and which are driven mainly by one.

#### Shared Interest Signals

A topic is "shared" when both people:
- Send longer-than-average messages on it
- Use excited emoji or exclamation marks from both sides
- Voluntarily bring it up across multiple conversations (not just responding)
- Reference it again in later conversations (callback)

**How to detect:**
- Look for threads where both people's messages are longer and more enthusiastic than their typical length
- Look for mutual questions and mutual answers on the same topic
- Look for topics that both people return to independently over time

#### One-Sided Interest Signals

A topic is "one-sided" when:
- One person brings it up repeatedly
- The other responds briefly, gives minimal follow-up questions, and does not introduce the topic independently
- The response messages are shorter than that person's average

Note the topic, who drives it, and how the other typically responds (brief acknowledgment, topic change, polite deflection).

#### Joint vs. Solo Activities

**Shared activities** — events or activities they have done or explicitly planned together:
- Look for: "我哋去咗", "上次一齊", "一齊去", "你帶我去", plans confirmed by both parties
- List each activity with approximate date and any quote evidence

**Solo activities** — activities each person does independently and reports back on:
- Look for: updates about activities the other did not participate in
- Note whether the other person showed interest or engaged minimally

#### Shared Vocabulary

Identify shorthand, inside references, or phrases used by both people:
- Words or phrases one person introduced that the other has adopted
- Shared nicknames for places, foods, activities, or third parties
- References to shared past experiences used as conversational shorthand
- Any non-standard spellings, abbreviations, or invented words unique to this pair

---

### Dimension 3: Inside Jokes & Recurring References

Identify humor and reference patterns that are specific to this relationship.

#### Recurring Phrases and Callbacks

Scan for:
- Any phrase, sentence, or expression that appears in multiple separate conversations (different dates)
- Expressions that appear to reference an earlier conversation or shared memory
- Reactions that suggest an established joke (e.g., one person says something and the other completes it, or immediately responds with a recognizable callback)

For each identified recurring element:
- Quote the phrase in the original language
- Note approximately how many times it appears across the date range
- Describe the apparent meaning or joke context if inferrable

#### Pet Names and Their Use

Extract all terms of endearment, nicknames, and affectionate address forms used between the two people.

**For each pet name:**
- Who uses it and who it refers to
- How frequently it appears (approximate count or density)
- Whether it appears consistently throughout the date range or evolves (e.g., a new nickname appears mid-history)
- Whether it's used in all contexts or only in affectionate/playful moments

#### Running Jokes

Look for jokes that have a recognizable punchline or structure that both people reference:
- A topic that always generates a specific kind of response
- Playful teasing patterns that recur (same subject, same dynamic)
- A repeated exaggeration or escalation pattern (e.g., mock complaints)

If no clear running jokes are detectable, note: `(no recurring jokes identified — may indicate limited chat history or humor expressed through stickers/emoji rather than text)`

---

### Dimension 4: Affection Patterns

Analyze how each person expresses care and closeness.

#### Goodnight / Good Morning

**Who says goodnight first more often?**
- Scan messages near the end of each conversation day for goodnight expressions in any language ("晚安", "night", "good night", "gn", "go sleep", "早啲訓")
- Count: partner says it first / user says it first / said simultaneously / varies
- Note any ritual phrasing (e.g., always followed by a specific emoji, always includes "love you" or similar)

**Who says good morning first more often?**
- Scan for morning openers ("早晨", "起身喇", "good morning", "早", "woke up")
- Note any pattern in who initiates the day's first message

#### Verbal Affection

Track direct statements of affection:
- Explicit love/miss statements: "我愛你", "I love you", "掛住你", "想你", "miss you", "好鍾意你"
- Expressions of happiness about the relationship: "好開心有你", "幸好有你", "好鍾意同你喺一齊"

For each type:
- Who says it more often?
- What contexts trigger it (out of nowhere, after a nice moment, before sleeping)?
- How does the other person respond?

#### Acts of Care

These are indirect affection signals — behaviors that show care without explicitly saying so.

**Health and wellbeing reminders:**
- "食咗飯未?" / "記得食飯" / "早啲訓" / "著多件衫" / "帶遮" / "你飲水未?"
- Track who sends these more often and in what contexts

**Checking in:**
- Unprompted messages asking how the other is doing
- Follow-up questions about something mentioned earlier (shows they were listening and remembered)

**Sharing daily life:**
- Sending photos of food, location, activities without being asked
- Narrating their day in detail as a way of including the other person
- Tagging the other person in things they would like

Note which person exhibits more of these indirect care behaviors and whether it is reciprocated.

#### Physical Affection Mentions

Text-based chats rarely capture this well, but flag any instances:
- References to hugs, kisses, holding hands, cuddling
- Mentions of wanting physical closeness or missing it ("好想抱你", "想攬你")
- Post-meetup messages reflecting on physical moments

If minimal: note `(physical affection not prominent in text — typical for text-based communication)`

#### LDR-Specific Affection Signals (activate when long-distance)

- **"Miss you" frequency and escalation:** Track "想你", "掛住你", "miss you" over time. Does frequency increase as separation lengthens? Is it initiated equally or one-sided?
- **Countdown language:** "仲有X日", "好快見到你", "X日後就見到你喇" — frequency and who initiates
- **Reunion anticipation vs. logistics:** When discussing upcoming visits, what's the ratio of emotional anticipation ("好期待", "好想見你") vs. practical logistics ("幾點機", "邊間酒店")?
- **Post-reunion emotional pattern:** After meeting in person, is there a warmth spike (lingering affection, "好唔捨得", "好掛住你") or a cool-down (shorter messages, lower frequency for a few days)?

---

### Dimension 5: Conflict Patterns

Analyze how tension and disagreement surface and resolve in this relationship.

#### How Disagreements Surface

Identify any moments where tone shifts negatively or tension is apparent.

**Signals to look for:**
- Sudden shortening of messages from one person (after previous longer messages)
- One-word or minimal replies ("ok", "哦", "係囉", "算") following a longer message
- Repeated questions that go unanswered or are deflected
- Messages that include mild criticism, complaint, or frustration markers ("點解你咁", "你成日", "唔係好")
- Passive phrasing ("隨便", "你話事", "無所謂", "你決定")
- Explicit expressions of hurt or annoyance ("我唔開心", "有少少唔開心", "係咁嫁")

Characterize the dominant mode:
- **Direct** — feelings or complaints stated plainly
- **Passive hints** — indirect language, short replies, visible mood drop without explicit statement
- **Humor-wrapped** — frustration expressed through jokes, sarcasm, or stickers
- **Cold withdrawal** — long gaps, minimal engagement, no explicit reason given

#### How They Resolve

Look for what happens after a tension episode:

- **One person apologizes** — look for "對唔住", "sorry", "係我唔好", "我錯喇"
- **Topic change** — conversation moves on to something neutral without explicit resolution
- **Sleep on it** — conversation goes quiet, resumes next day with normal tone
- **Humor to de-escalate** — one person makes a joke, sends a funny sticker, shifts energy
- **Explicit resolution** — both acknowledge the issue and discuss it to a clear endpoint

Note: Who is more often the first to reach out after a conflict? Who tends to apologize first?

#### Recovery Time

Estimate how quickly the conversation tone returns to normal after a tension episode:
- Immediate (same message thread, within minutes)
- Same day (later in the day, after a gap)
- Next day (conversation resumes normally on the following day)
- Extended (more than one day before tone normalizes)

#### LDR-Specific Conflict Signals (activate when long-distance)

- **Distance-triggered tension:** Do conflicts cluster around long separation stretches or around failed visit plans?
- **Jealousy or trust signals:** "你同邊個", "你去咗邊", "點解唔聽電話" — frequency, escalation pattern, and resolution
- **Timezone/availability frustration:** Complaints about one person being unavailable when the other needs to talk, mismatched schedules causing friction
- **Reunion pressure:** Do conflicts spike around reunions (expectations too high, logistics stress)?

If no conflicts are visible in the chat history, note: `(no conflict observed in chat history — may indicate early relationship stage, or conflict happens offline, or this is an exceptionally harmonious period)`

---

### Dimension 6: Relationship Health Signals

Assess the overall trajectory and depth of the relationship.

#### Engagement Trend

Examine how message frequency and conversational energy change across the date range.

**How to assess:**
- Divide the date range into thirds (early period, middle period, recent period)
- Compare approximate message frequency per week in each period
- Compare average message length and emoji density per period
- Note any significant drops or spikes and whether they correlate with external context (busy periods mentioned, trips, illness)

**Characterize the trend as one of:**
- **Increasing** — more frequent, longer, more engaged over time
- **Stable** — consistent engagement throughout
- **Decreasing** — less frequent or shorter messages over time
- **Variable** — fluctuates, with identifiable reasons if possible

#### Topic Diversity

Assess whether the conversation is deepening or staying surface-level.

**Surface-level signals:**
- Primarily logistics and scheduling
- Mostly sharing external content (links, memes) without personal commentary
- Repetitive daily check-in format without variation

**Deepening signals:**
- Discussions of feelings, fears, dreams, values
- Conversations about the future — individual goals or shared plans
- Sharing vulnerabilities or worries
- Asking the other person for advice or opinions on meaningful personal decisions
- Talking about family, childhood, formative experiences

Characterize as: surface-level, developing, or deep — with evidence.

#### Future Planning

Look for any mentions of future activities, experiences, or life plans together.

**Signals:**
- Specific plans: "下次去X", "第時帶你食", "我哋去X", "下個月一齊去"
- Aspirational mentions: "好想同你去X", "有日帶你去試下"
- Life milestone references: living arrangements, travel plans, meeting family/friends

Note each instance with approximate date and the specific quote.

#### Vulnerability

Assess whether either person has shared worries, insecurities, or deeper feelings.

**Signals:**
- Sharing anxieties about work, health, relationships, family
- Expressing self-doubt or admitting struggle
- Asking for emotional support or reassurance
- Sharing something personal they explicitly flag as unusual ("我唔係好鍾意同人講呢啲", "我唔知點解我咁", "其實我有少少擔心")

Note who is more vulnerable in conversation and whether the other person responds with support or pivots away.

---

### Dimension 7: Relationship Health Indicators

This dimension assesses the health of the couple's communication and emotional connection patterns. Focus on observable behaviors in the chat — not character assessments.

#### Destructive Communication Patterns

Scan all conflict episodes and tense exchanges for four specific harmful patterns:

**Criticism (attacking character, not behavior):**
- Generalizations: "你成日咁", "你從來都唔會", "你每次都係咁"
- Character attacks: "你好自私", "你根本唔在乎"
- Distinguish from legitimate complaint (about a specific behavior) vs. criticism (about who the person is)

**Contempt (superiority, mockery, dismissiveness):**
- Sarcasm or mockery during conflict: "哦，好叻呀你", "你識咩"
- Eye-roll emoji (🙄) or dismissive stickers used during tense exchanges
- Condescending tone: explaining things as if the other person is stupid
- This is the most corrosive pattern — flag even isolated instances

**Defensiveness (deflecting blame, counter-complaining):**
- "係你先..." / "你自己都..."
- Responding to a complaint with a complaint instead of acknowledging
- "我邊有" / "唔係我" pattern when confronted
- Playing victim: "你成日怪我"

**Stonewalling (complete emotional shutdown):**
- Long unexplained silences mid-conflict (different from normal offline gaps)
- Conversation-enders that shut down discussion: "算啦", "隨便你", "無嘢講", "你鍾意啦"
- One-word replies during active conflict: "哦", "好", "知道"
- Physically leaving the conversation (going offline) during unresolved tension

**For each pattern detected:**
- Who exhibits it (partner, user, or both)
- Frequency: isolated (1-2 instances), occasional (3-5), recurring (6+)
- Severity: mild (brief, quickly corrected) vs. escalated (sustained, unanswered)
- Original-language quote evidence
- Whether the other person escalates or de-escalates in response

If no destructive patterns detected: `(no destructive communication patterns detected in available chat history)`

#### Repair Attempts

Scan for efforts to de-escalate or reconnect during or after conflict episodes:

**Types of repair attempts:**
- **Humor:** Using a joke, funny sticker, or playful tone to break tension
- **Direct acknowledgment:** "我知我啱啱...", "sorry我唔應該咁講"
- **Affection bridge:** Switching to warmth mid-conflict — pet name, "我仲係好鍾意你"
- **Topic bridge:** Moving to safe ground, then circling back to the issue later with calmer tone
- **Meta-communication:** Naming the dynamic — "我哋唔好嗌", "我哋傾下先"
- **Responsibility-taking:** "係我唔好", "我下次會注意"

**Assess:**
- Who initiates repairs more often (partner / user / balanced)
- Primary repair style (which type above is most common)
- Repair success rate: when a repair is attempted, does the other person accept (engage, soften) or reject (ignore, escalate)?
- Speed: are repairs attempted during the conflict, same day, or next day?

If no conflict episodes to assess: `(no conflict episodes observed — cannot assess repair patterns)`

#### Emotional Connection Assessment (A.R.E.)

Assess three dimensions of emotional bond quality:

**Accessible — "Can I reach you?"**
- When one person sends an emotional bid (sharing a worry, excitement, need), does the other respond?
- Look for unanswered emotional messages — not logistics (those are normal to miss), but genuine emotional reach-outs that get ignored or deflected
- Score: High (emotional bids consistently answered), Medium (sometimes missed), Low (frequently ignored)
- Evidence for each score

**Responsive — "Can I rely on you emotionally?"**
- When one person shares vulnerability (worry, insecurity, sadness), does the other engage with empathy or pivot away?
- Look for: active listening signals (asking follow-up questions about feelings), validation ("我明白", "正常嘅"), vs. fixing/dismissing ("唔好咁諗", "冇事啦")
- Score: High / Medium / Low for each person
- Evidence for each score

**Engaged — "Do I know you're there with me?"**
- Does the conversation show active interest in each other's inner world — dreams, fears, opinions, growth?
- Or is it primarily logistics, scheduling, and surface-level updates?
- Look for: unsolicited questions about the other's feelings, remembering and following up on previously shared concerns, sharing observations about the other's mood
- Score: High / Medium / Low for each person
- Evidence for each score

---

## Evidence Standard

Apply these rules across all 7 dimensions:

- **Every finding must reference specific chat evidence.** Either quote the message directly (in original language, never translate) or describe a pattern with enough detail to be verifiable.
- **Confidence tags are mandatory for pattern-based claims:**
  - `(strong -- 5+ instances)` — pattern appears reliably across multiple conversations and dates
  - `(moderate -- 2-4 instances)` — pattern appears a few times, enough to suggest a tendency
  - `(weak -- 1 instance)` — observed only once, may not be representative
- **Counts and ratios:** Where quantitative claims are made (initiation ratio, message volume), provide approximate numbers or ranges. Do not guess precisely if the data is ambiguous — round to the nearest meaningful estimate and note uncertainty.
- **Insufficient data:** If a dimension has too little evidence to make any claim, write: `(insufficient data — need more chat history)`. Do not fabricate or over-infer.
- **Use original language for all quotes.** Do not translate quotes to English or any other language.

---

## Output Format

Structure the analysis as follows. This output feeds directly into the `dating_ideas_builder` and provides context for gift recommendations. Include all sections even if a section's finding is "insufficient data" or "not observed."

```
## LDR Status
- Detected: [yes / no / suspected]
- Evidence: [if yes or suspected — what signals]
- Confirmed by user: [yes / no / pending]

---

## Dimension 1: Communication Patterns

### Initiation Ratio
- Partner-initiated: [X]% / User-initiated: [Y]% (approx. [N] total conversation sessions)
- Pattern notes: [any shift over time, context-dependent variation]

### Response Speed
- Partner: [characterization — e.g., "typically replies within 5-15 min; longer gaps during work hours"]
- User: [characterization]
- Notable asymmetry: [if any]

### Peak Hours
- Most active period: [time block(s)]
- Goodnight pattern: [who says it first — evidence]
- Good morning pattern: [who initiates — evidence]
- Weekend vs. weekday: [any shift]

### Message Volume Balance
- Total message counts: Partner [N] / User [N] (approx.)
- Dominant conversational contributor: [partner / user / roughly equal]
- Notes on imbalance: [context or trend]

### Conversation Starters
- Partner typically opens with: [topic category] — example: "[original quote]"
- User typically opens with: [topic category] — example: "[original quote]"

---

## Dimension 2: Shared vs. Individual Interests

### Shared Topics (both engage enthusiastically)
- [Topic]: [evidence of mutual engagement]
- [...]

### One-Sided Topics
- [Topic] — driven by [partner/user]: [evidence of asymmetry]
- [...]

### Joint Activities
- [Activity] — [approximate date/context] — "[quote if available]"
- [...]

### Solo Activities Reported
- Partner: [list with brief descriptions]
- User: [list with brief descriptions]

### Shared Vocabulary
- "[phrase/word]" — [origin or context, frequency]
- [...]

---

## Dimension 3: Inside Jokes & Recurring References

### Recurring Phrases
- "[phrase]" — appears [N] times across date range — [context/meaning]
- [...]

### Pet Names
- [Name] — used by [who] for [who] — frequency: [characterization] — context: [description]
- [...]

### Running Jokes
- [Description of joke/pattern] — "[example quote]" — recurs [N] times
- [or: (no recurring jokes identified — [reason])]

---

## Dimension 4: Affection Patterns

### Goodnight / Good Morning
- Goodnight first: [partner / user / varies] — example: "[quote]"
- Good morning first: [partner / user / varies] — example: "[quote]"

### Verbal Affection
- Who expresses more often: [partner / user / roughly equal]
- Primary expressions used: [list in original language]
- Context: [when it tends to appear]

### Acts of Care
- Health/wellbeing reminders: [who sends more — examples in original language]
- Checking in unprompted: [characterization — who, how often]
- Sharing daily life: [characterization — who shares more, what they share]

### Physical Affection Mentions
- [instances if any, or: (physical affection not prominent in text — typical for text-based communication)]

---

## Dimension 5: Conflict Patterns

### How Conflict Surfaces
- Primary mode: [direct / passive hints / humor-wrapped / cold withdrawal]
- Evidence: [description of observed episode(s), or absence note]

### How They Resolve
- Primary resolution pattern: [characterization]
- Who reaches out first after conflict: [partner / user / varies]
- Who apologizes first: [partner / user / varies / not observed]

### Recovery Time
- Typical: [immediate / same day / next day / extended]
- Evidence: [example if available]

### Conflict Overall
- [Summary observation, or: (no conflict observed in chat history — may indicate early relationship stage or conflict happens offline)]

---

## Dimension 6: Relationship Health Signals

### Engagement Trend
- Trend: [increasing / stable / decreasing / variable]
- Early period: [characterization]
- Middle period: [characterization]
- Recent period: [characterization]
- Notes: [any contextual factors]

### Topic Diversity
- Level: [surface-level / developing / deep]
- Evidence of surface topics: [examples]
- Evidence of deeper engagement: [examples or absence]

### Future Planning
- "[quote]" — [date context] — [what was being planned]
- [or: (no future planning observed)]

### Vulnerability
- More vulnerable: [partner / user / roughly equal]
- Evidence: [examples in original language, or (insufficient data)]
- Other person's response style: [supportive / pivots away / mixed]

---

## Dimension 7: Relationship Health Indicators

### Destructive Patterns
- Criticism: [not detected / detected — who, frequency, severity]
  Evidence: "[original-language quote]"
- Contempt: [not detected / detected — who, frequency, severity]
  Evidence: "[original-language quote]"
- Defensiveness: [not detected / detected — who, frequency, severity]
  Evidence: "[original-language quote]"
- Stonewalling: [not detected / detected — who, frequency, severity]
  Evidence: "[original-language quote]"
- Overall: [no destructive patterns / isolated instances / concerning recurring pattern]

### Repair Attempts
- Who repairs more: [partner / user / balanced / not observed]
- Primary repair style: [humor / direct acknowledgment / affection bridge / topic bridge / meta-communication / responsibility-taking]
- Repair success rate: [high / medium / low — with description]
- Speed: [during conflict / same day / next day]
- Evidence: "[quote or pattern description]"

### A.R.E. Assessment
| Dimension | Partner | User | Evidence |
|-----------|---------|------|----------|
| Accessible | High/Med/Low | High/Med/Low | [brief evidence] |
| Responsive | High/Med/Low | High/Med/Low | [brief evidence] |
| Engaged | High/Med/Low | High/Med/Low | [brief evidence] |

### Overall Health
- [1-2 sentence synthesis: strengths and concerns]
```

---

## Final Checklist

Before outputting the analysis, verify:

- [ ] Initiation ratio is based on actual session counting, not impression
- [ ] Response speed estimates exclude obvious sleep/offline gaps (gaps > 12 hours)
- [ ] All quotes are in the original language (no translations)
- [ ] Conflict section explicitly states if no conflict was observed and gives the standing note
- [ ] Future planning quotes are pulled verbatim from the chat
- [ ] Engagement trend is assessed across at least two periods (early vs. recent), not just overall impression
- [ ] Every claim has a confidence tag or is marked as (insufficient data)
- [ ] Output explicitly notes when a section has no observable data rather than omitting it
- [ ] LDR status is assessed (detected / not detected / suspected) with evidence if applicable
- [ ] If LDR detected, Dimensions 1, 4, 5 include LDR-specific signal analysis
- [ ] Dimension 7 destructive patterns are assessed individually (all four checked, not just a general statement)
- [ ] Dimension 7 repair attempts cite specific episodes or note absence
- [ ] A.R.E. assessment scores both people separately with evidence per dimension
- [ ] No psychological labels — behaviors described, not diagnosed
- [ ] Destructive patterns framed neutrally (frequency + evidence, not character judgments)

This output is passed to the `dating_ideas_builder` (to calibrate date idea intimacy and activity level) and informs the `gifts_builder` (to surface relationship-stage-appropriate recommendations). Be specific — gaps here reduce the quality of downstream outputs.
