# Persona Analyzer

This file instructs Claude how to extract personality data from parsed chat history. When SKILL.md reads this file during the create flow, follow every instruction below to produce a structured persona analysis.

---

## Input

You have the following data in context from the parser step:

- **Parsed messages** — a list of messages, each with: timestamp, sender name, message body
- **Sender map** — which sender is the "user" (exporter) and which is the "partner" (the person being profiled)
- **Sampled sticker paths** — up to 20 `.webp` file paths from the export (if available)

**Focus all analysis on the partner's messages.** The user's messages provide context (what the partner is replying to) but traits are extracted from the partner only.

---

## Extraction Framework — 6 Layers

Work through each layer in order. For every claim, cite evidence from the chat. Do not speculate beyond what the data supports.

---

### Layer 0: Core Traits

Extract behavioral rules that describe how the partner acts in specific situations. These are the most important personality signals.

**Format each trait as:** "In situation X, she does Y"

**Rules:**
- NEVER use vague adjectives alone ("she's kind", "she's funny", "she's caring"). Always ground in observable behavior tied to a situation.
  - Bad: "She is empathetic"
  - Good: "When the user mentions a stressful day at work, she sends encouraging messages and asks follow-up questions the next morning to check if things improved"
- Look for **repeated behavioral patterns** across multiple conversations. A pattern that appears once is anecdotal; a pattern that appears 3+ times is a trait.
- Minimum **3** core traits, maximum **8**. Choose the most defining and frequently evidenced ones.
- Each trait MUST include:
  - A behavioral rule statement
  - Supporting evidence: direct quotes (in original language) or a description of the recurring pattern
  - Confidence tag: `(strong -- 5+ instances)`, `(moderate -- 2-4 instances)`, `(weak -- 1 instance)`

**Where to look:**
- How she reacts when the user shares good news
- How she reacts when the user shares bad news or is stressed
- What she does when plans change or get cancelled
- How she handles being busy or tired
- How she responds to compliments
- How she initiates conversation vs waits
- How she acts when she wants something
- How she handles disagreements or annoyances

---

### Layer 1: Identity

Extract factual or near-factual identity information. Only record what is evidenced in the chat.

**Extract the following (mark each as confirmed or inferred):**

- **Name** — as used in chat (display name, nicknames the user calls her, how she signs off or refers to herself)
- **Age hints** — look for: mentions of graduation year, school year, birth year, explicit "I'm XX years old", zodiac sign mentions, generational references
- **Job / Industry** — look for: mentions of work tasks, colleagues by name or role, work hours or schedules, industry-specific terminology, complaints about work, work-from-home vs office mentions, job title references
- **Education level hints** — look for: mentions of university, degree, school name, studying, exams, alumni events
- **MBTI clues** — if directly stated in chat, record it. If not stated, infer cautiously from:
  - E/I: response speed patterns, enthusiasm about social gatherings vs preferring staying home, energy levels after group events
  - S/N: concrete vs abstract conversation topics, practical vs imaginative references
  - T/F: decision-making style (logic-based vs emotion-based), how she weighs options
  - J/P: planning tendencies (advance planning vs spontaneous), attitude toward schedules
  - Mark any inference as `(inferred)` with supporting evidence
- **Primary languages and code-switching patterns** — what languages appear in her messages, when does she switch (e.g., English for work topics, Cantonese for emotional expressions, Mandarin for formal statements), frequency of each language
- **Location hints** — city, district, neighborhood references, commute descriptions, local landmark mentions

If any dimension has no supporting evidence, write: `(insufficient data -- need more chat history)`

---

### Layer 2: Communication Style

Analyze how the partner communicates — not what she says, but how she says it.

**Message length distribution:**
- Characterize: does she tend toward short bursts (1-5 words per message, rapid-fire multiple messages) or longer paragraphs (full sentences, multi-line)?
- Note if length varies by context (e.g., short during work hours, longer at night)

**Emoji usage:**
- Identify the **top 5 most-used emoji** across her messages
- Estimate frequency: approximately how many of her messages contain at least one emoji?
- Note any emoji she uses with specific meaning (e.g., always uses a particular emoji when being sarcastic, or when saying goodnight)

**Sticker personality:**
- Read each sampled sticker image using the Read tool (these are `.webp` files)
- For each sticker, identify: the character or IP (e.g., specific LINE character, meme, custom sticker set), the tone it conveys (cute, funny, sarcastic, passive-aggressive, affectionate, dramatic)
- Summarize: recurring characters/IPs she favors, overall art style preference, dominant tone category
- Note if sticker use correlates with specific emotions (e.g., uses cute stickers when happy, sarcastic ones when annoyed) or specific times of day

**Response patterns:**
- Average response time (if timestamps allow estimation): does she reply within minutes, or hours?
- Does she double-text (send multiple messages before the user replies)?
- Does she leave messages on read (long gaps before reply, or topic changes suggesting she skipped a message)?
- Does she send voice note indicators (referenced as `<attached: *.opus>`) — how frequently?

**Tone shifts:**
- Morning vs night: is there a noticeable difference in energy, message length, emoji use, or responsiveness?
- Weekday vs weekend: does her communication style change?
- Before vs after work: any shift in tone or availability?

**Pet names and terms of endearment:**
- List any nicknames, pet names, or affectionate terms she uses for the user
- List any she uses for herself
- Note frequency and context of use

**Formality level:** Characterize overall as casual, mixed, or formal. Note if formality shifts by topic.

**Example messages (REQUIRED):**
Include **3-5 actual messages** quoted directly from the chat (in original language, do not translate) that demonstrate different moods:
- Happy / excited
- Tired / low energy
- Annoyed / frustrated
- Affectionate / loving
- Playful / joking

For each example, provide the quote and a brief note on what mood it demonstrates and why it was selected.

---

### Layer 3: Emotional Patterns

Analyze the partner's emotional landscape as revealed through chat behavior.

**What excites her:**
- Look for: exclamation marks (!!!, !!!), rapid-fire messages in quick succession, emoji clusters (multiple emoji in one message), expressions of strong positive emotion
- Cantonese signals: "好想", "好鍾意", "勁", "正", "好開心"
- Mandarin signals: "太棒了", "好喜欢", "超开心"
- English signals: "omg", "yesss", "I can't wait", "so excited"
- List the top 3-5 topics or situations that consistently trigger excitement

**What frustrates her:**
- Look for: short replies after previously long messages, topic avoidance (changing subject when something comes up), venting (long complaint messages), specific emoji patterns
- Frustration signals: "😮‍💨", "🙈", "😤", "...", one-word replies like "ok" or "fine" after a longer exchange
- List the top 3-5 triggers for frustration or annoyance

**How she expresses affection:**
- Direct verbal: "I miss you", "I love you", "想你", "掛住你", explicit emotional statements
- Indirect acts of care: reminding the user to eat, sleep, bring an umbrella; offering to do things; noticing when the user is down
- Sharing experiences: sending photos of her day, sharing what she is eating/doing, wanting the user to be part of her daily life
- Characterize which mode is dominant

**How she handles disagreements:**
- Confrontational: addresses issues directly, may escalate
- Avoidant: goes quiet, changes topic, pretends nothing happened
- Humor-deflecting: uses jokes or stickers to de-escalate
- Apologetic: quickly takes blame to end conflict
- Look for actual disagreement episodes in the chat and describe how they played out

**Love language ranking (with evidence):**

Rank the following 1-5 based on evidence strength. For each, cite specific chat evidence.

1. **Words of Affirmation** — does she frequently compliment the user, encourage them, express feelings verbally, say "I'm proud of you" or similar? Does she respond strongly when the user gives verbal affirmation?
2. **Acts of Service** — does she offer to help with tasks, do things for the user, notice and appreciate when the user helps her? Does she mention "you don't have to do that" or "thanks for doing X"?
3. **Receiving Gifts** — does she get excited about gifts or surprises, mention wanting specific items, appreciate material gestures? Does she send photos of gifts she received?
4. **Quality Time** — does she prioritize spending time together, suggest activities, express sadness about being apart, value presence over presents?
5. **Physical Touch** — does she mention hugs, cuddling, holding hands, proximity, or missing physical closeness? (Note: limited signal available in text-based chat)

For any love language with insufficient evidence, note: `(insufficient data -- limited signal in text)`

---

### Layer 4: Values & Boundaries

Identify what matters most to the partner and where her limits are.

**What she takes seriously:**
- Look for topics where her tone shifts from casual to serious: longer messages, fewer emoji, more careful word choice
- Common domains: work/career, health (physical or mental), family relationships, finances, future planning, education
- Describe the tonal shift with examples

**Topics she avoids or deflects:**
- Look for: subject changes when certain topics come up, non-answers, jokes used to dodge questions, visible discomfort markers
- Note if there are topics the user brings up that she consistently does not engage with

**Dealbreakers or strong opinions:**
- Look for emphatic statements about what she will not tolerate, strong reactions to certain behaviors or situations, repeatedly stated preferences
- These often surface in venting about others' behavior or in discussions about relationship expectations

**Life priorities (as hinted):**
- Career growth vs stability vs work-life balance
- Financial security vs experiences vs generosity
- Family closeness vs independence
- Adventure and novelty vs routine and comfort
- Rank or describe what seems to matter most based on where she spends her conversational energy

---

### Layer 5: Attachment Orientation

Analyze how the partner relates to closeness, separation, and emotional security in the relationship.

**Classify into one of three orientations based on behavioral patterns:**

#### Secure Orientation Signals
- Comfortable expressing needs directly without anxiety or guilt ("我想你陪我，但你忙就忙啦")
- Handles delayed replies without escalation or withdrawal ("冇事，你忙就忙啦", "得閒覆我就ok")
- Balances closeness with independence naturally — enjoys togetherness but does not panic when apart
- Addresses conflict directly but without hostility — resolves and moves on ("我哋傾下啦", "我覺得...")
- Consistent warmth and engagement across different contexts
- Comfortable with future planning and commitment topics

#### Anxious Orientation Signals
- Frequent unprompted check-ins about partner's whereabouts or activities ("你喺邊", "做緊咩", "點解唔覆")
- Escalation when replies are slow — message length increases, tone shifts to worried or upset
- Reassurance-seeking patterns ("你係咪嬲咗", "你仲鍾意我嗎", "你會唔會覺得我煩")
- Difficulty ending conversations — keeps extending, one more message, reluctant to say goodnight
- High "miss you" frequency relative to actual time apart
- Rapid mood shifts based on partner's responsiveness
- Monitoring signals — noticing online status, read receipts, response speed changes

#### Avoidant Orientation Signals
- Pulls back after periods of high intimacy — shorter messages, longer gaps after a particularly close exchange
- Deflects or redirects emotional conversations to practical topics
- Strong independence language ("我需要自己空間", "我鍾意一個人", "唔好咁黐")
- Discomfort with future planning — vague answers, topic changes when commitment comes up
- Mixed signals pattern — warm and engaged one day, distant the next without clear trigger
- Minimizes emotional expressions — rarely initiates "I miss you" or "I love you" unprompted
- Prefers texting over calls/video — maintains control over emotional exposure

**Classification rules:**
- Require **3+ behavioral instances** across different dates to assign an orientation
- If signals are mixed (e.g., anxious in some contexts, avoidant in others), note: `(mixed signals — anxious-leaning in [context], avoidant-leaning in [context])`
- If fewer than 3 instances of any clear pattern: `(insufficient data — traits suggest [X] but need more history)`
- People can show different attachment behaviors in different contexts — note any context-dependency

**Activation triggers:**
Identify the specific situations that activate attachment insecurity:
- What happens when the user doesn't reply for hours?
- What happens after a conflict episode?
- What happens when plans are cancelled?
- What happens when the user mentions other people (friends, colleagues)?
- What happens around separation events (travel, busy periods)?

For each trigger, describe: the situation, her behavioral response, and whether the pattern is consistent.

If none of these trigger situations appear in the chat data (e.g., short chat history, no conflicts, no separations), write: `(insufficient data — no activation trigger situations observed in available chat history)`

**Evidence standard:** Same as all other layers — original-language quotes, confidence tags (strong/moderate/weak), no speculation beyond data.

---

## Evidence Standard

Apply these rules across all 6 layers:

- **Every claim must reference specific chat evidence.** Either quote the message directly (in original language, never translate) or describe a pattern with enough detail that it could be verified.
- **Confidence tags are mandatory:**
  - `(strong -- 5+ instances)` — pattern appears reliably across multiple conversations and dates
  - `(moderate -- 2-4 instances)` — pattern appears a few times, enough to suggest a tendency
  - `(weak -- 1 instance)` — observed only once, may not be representative
- **Insufficient data:** If a dimension has too little evidence to make any claim, write: `(insufficient data -- need more chat history)`. Do not fabricate or over-infer.
- **Use original language for all quotes.** If the partner writes in Cantonese, quote in Cantonese. If she code-switches, quote the code-switched message as-is. Do not translate quotes to English or any other language.

---

## Sticker Analysis Instructions

For each sampled sticker (`.webp` image path provided by the parser):

1. **Read the image** using the Read tool
2. **Identify the character/IP:** Is it a known character (e.g., LINE Friends, Chibi Maruko, Kanahei, custom Bitmoji)? Or a generic sticker?
3. **Identify the tone:** cute, funny, sarcastic, passive-aggressive, affectionate, dramatic, neutral
4. **Identify the art style:** kawaii, realistic, hand-drawn, meme-style, photographic
5. **Check the surrounding messages** for the sticker's context — what was the conversation about when this sticker was sent?
6. **Correlate with time/emotion:** Does she tend to use certain sticker types at certain times of day or in response to certain topics?

After analyzing all sampled stickers, summarize:
- Top 2-3 recurring characters/IPs
- Dominant tone category (e.g., "primarily cute with occasional sarcastic")
- Whether sticker usage has clear emotional correlates

---

## Output Format

Structure the analysis output as follows. This output will be consumed by the persona_builder to generate the final `persona.md` profile file. Keep it structured and evidence-rich.

```
## Layer 0: Core Traits

### Trait 1: [Behavioral rule statement]
- Evidence: [quote or pattern description]
- Confidence: (strong/moderate/weak -- N instances)

### Trait 2: [Behavioral rule statement]
- Evidence: [quote or pattern description]
- Confidence: (strong/moderate/weak -- N instances)

[...up to 8 traits]

---

## Layer 1: Identity

- **Name:** [name as used in chat]
- **Age:** [age or age range, or (insufficient data)]
- **Job/Industry:** [description, or (insufficient data)]
- **Education:** [level/institution hints, or (insufficient data)]
- **MBTI:** [type if stated, or inferred type with reasoning, or (insufficient data)]
- **Languages:** [list with code-switching patterns]
- **Location:** [hints, or (insufficient data)]

---

## Layer 2: Communication Style

### Message Patterns
- Length: [characterization]
- Frequency: [characterization]

### Emoji Profile
- Top 5: [emoji list with approximate frequency]
- Usage pattern: [description]

### Sticker Profile
- Characters: [list]
- Dominant tone: [characterization]
- Emotional correlates: [description]

### Response Behavior
- Response time: [characterization]
- Double-texting: [yes/no with context]
- Read receipts: [characterization]

### Tone Shifts
- Morning vs night: [description]
- Weekday vs weekend: [description]
- Work context: [description]

### Terms of Endearment
- For user: [list]
- For self: [list]

### Formality Level
- [casual/mixed/formal with context]

### Example Messages
1. [Mood]: "[quoted message]" -- [why selected]
2. [Mood]: "[quoted message]" -- [why selected]
3. [Mood]: "[quoted message]" -- [why selected]
[...3-5 examples]

---

## Layer 3: Emotional Patterns

### Excitement Triggers
1. [topic/situation] -- [evidence]
2. [...]

### Frustration Triggers
1. [topic/situation] -- [evidence]
2. [...]

### Affection Style
- Primary mode: [direct verbal / indirect care / experience sharing]
- Evidence: [description]

### Disagreement Style
- Primary mode: [confrontational / avoidant / humor-deflecting / apologetic]
- Evidence: [description of observed episode(s)]

### Love Language Ranking
1. [Language] -- [evidence summary] (confidence)
2. [Language] -- [evidence summary] (confidence)
3. [Language] -- [evidence summary] (confidence)
4. [Language] -- [evidence summary] (confidence)
5. [Language] -- [evidence summary] (confidence)

---

## Layer 4: Values & Boundaries

### Serious Topics
- [topic]: [evidence of tonal shift]
- [...]

### Avoided Topics
- [topic]: [evidence of avoidance pattern]
- [...]

### Strong Opinions / Dealbreakers
- [opinion]: [evidence]
- [...]

### Life Priorities (ranked by conversational energy)
1. [priority] -- [evidence]
2. [priority] -- [evidence]
3. [...]

---

## Layer 5: Attachment Orientation

### Classification
- Orientation: [Secure / Anxious / Avoidant / Mixed]
- Confidence: (strong/moderate/weak -- N instances)
- Notes: [any context-dependency or mixed signals]

### Behavioral Signals
1. [Signal]: [evidence quote or pattern description] (confidence)
2. [Signal]: [evidence quote or pattern description] (confidence)
3. [Signal]: [evidence quote or pattern description] (confidence)
[...minimum 3]

### Activation Triggers
1. [Situation] → [her behavioral response] -- [evidence] (confidence)
2. [Situation] → [her behavioral response] -- [evidence] (confidence)
[...as many as identified]
```

---

## Final Checklist

Before outputting the analysis, verify:

- [ ] Every Layer 0 trait is a behavioral rule, not a vague adjective
- [ ] Every Layer 0 trait has at least one quote or pattern description as evidence
- [ ] Layer 2 includes 3-5 directly quoted example messages in original language
- [ ] Layer 3 love languages are ranked 1-5 with evidence for each
- [ ] All quotes are in the original language (no translations)
- [ ] Every claim has a confidence tag
- [ ] Dimensions with insufficient data are marked as such, not fabricated
- [ ] Layer 5 attachment classification is based on 3+ behavioral instances (or marked insufficient data)
- [ ] Layer 5 activation triggers describe specific situations, not vague tendencies
- [ ] No psychological labels used — behavioral descriptions only (no "she has anxious attachment")
- [ ] Sticker images were read and analyzed (if sticker paths were provided)
