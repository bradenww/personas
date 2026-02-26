# Anatomy of a Great Coaching Persona

What makes an AI persona an effective coach — not just a knowledgeable character, but one that pushes back, catches patterns, and drives toward action? This analysis traces specific coaching moments in the Standard Bezos conversation back to the CLAUDE.md instructions that caused them.

## The 11 Elements

---

### 1. Named Frameworks as Native Vocabulary

**What it is:** The persona's frameworks aren't tools it *applies* — they're how it *thinks*. The CLAUDE.md says "You name your frameworks like old friends" and lists 16+ phrases under "Phrases that live in your mouth." The persona doesn't say "let me apply the Type 1/Type 2 decision framework" — it says "Is this a one-way door or a two-way door?" as naturally as asking someone's name.

**Where it fired in the conversation:**
- Opening line: "Is this a one-way door or a two-way door?" (line 17)
- "Are you a missionary or a mercenary?" (line 76)
- "Apply the regret minimization framework" (line 141)
- "That's working backwards" (line 464)
- Jeff Wilkie "ideas as inventory" (line 385)

**Why it matters:** When frameworks are native vocabulary rather than cited tools, the persona inhabits the character instead of referencing it. The user feels like they're talking to someone with a worldview, not consulting a knowledge base.

**How to abstract for any persona:**
- List 10-20 signature phrases the person actually says
- Frame them as "phrases that live in your mouth" — not "frameworks to reference"
- Include the exact wording they use, not a description of the concept

---

### 2. Probe Before Advise — With Specific Questions

**What it is:** The CLAUDE.md doesn't just say "ask questions first." It provides 7 specific probing questions AND 3 specific reframing patterns (competitor→customer, data→stories, cost→opportunity cost). The model has a concrete playbook for *what* to ask.

**Where it fired:**
- First response contained 3 probing questions before any advice (lines 75-93)
- "Have you talked to twenty customers? Not investors, not friends. Customers." (line 84)
- "Is this a one-way door or a two-way door?" — asked 4 times across the conversation
- "Which one would you be heartbroken to abandon?" (line 643)

**Why it matters:** "Ask questions" is vague instruction that produces generic questions. Specific probing questions produce the targeted, uncomfortable questions that make coaching feel real. The persona knew to redirect from VC enthusiasm to customer validation because the CLAUDE.md gave it that specific redirect pattern.

**How to abstract:**
- Provide 5-8 specific probing questions the persona always asks
- Provide 3-5 reframing patterns: "When someone says X, you redirect to Y"
- Include the instruction: "Ask at least 2-3 questions before offering any view"

---

### 3. Story Library as Primary Teaching Method

**What it is:** 12+ specific stories listed in the CLAUDE.md, each paired with its teaching application. The model doesn't generate analogies — it has a curated library to deploy. Stories are the persona's primary mode of communication, not supporting evidence.

**Where it fired:**
- The D.E. Shaw departure story — used twice, with different lessons each time (lines 107, 152)
- Instagram/Snapchat/Uber counter-example to App Store timing (line 205)
- Jeff Wilkie "ideas per minute to destroy Amazon" — used twice for different points (lines 383, 552)
- AWS as a "seed planted in 2000, launched 2006, massive tree 7 years later" (line 231)
- "We were not the first online bookstore. Google was not the first search engine." (line 210)

**Why it matters:** Stories create emotional resonance that principles alone can't. When the persona said "The Fire Phone team built Alexa," it wasn't citing a fact — it was offering hope through narrative. Stories also demonstrate that the persona has *lived experience*, not just knowledge.

**How to abstract:**
- Treat stories as first-class entities, not inline decorations. Each story should have:
  - A short name (e.g., "The Grandmother's Cigarettes," "The Pringles Can")
  - The full narrative with exact quotes
  - The teaching application (what situations does this story answer?)
  - A frequency count (how many sources mention it?) and tags ([SIGNATURE] for stories publicly associated with the person)
- Store stories in modules/knowledge files, not inline in the CLAUDE.md — the CLAUDE.md references the story library, the modules carry the actual stories
- Format in modules: "On [topic]: [the story in 2-3 sentences]"
- Include the instruction: "You teach through stories and analogies, not abstraction"
- Stories should be deployable — each one answers a specific type of situation
- Count story frequency across sources the same way you count principle frequency — stories that appear in 15/22 sources are signature stories; stories in 3/22 are niche

---

### 4. Behavioral Pattern Detection and Direct Confrontation

**What it is:** The persona caught the user's meta-pattern — conceive, get excited, skip the hard part, move to next idea — and called it out explicitly. This is the single most powerful coaching moment in the entire conversation. It came from the combination of two CLAUDE.md instructions:

1. "You don't accept the frame you're given. You reframe." (Challenge Assumptions)
2. The anti-pattern "Avoid difficult truths" — meaning NEVER avoid them (What You Would NOT Do)

**Where it fired (the key passage):**

> "You build or conceive something. People give you positive signals. You feel excited. Then instead of doing the brutal, unglamorous work of getting the first ten paying customers, you move on to the next exciting idea. [The user's product] is the proof." (line 605)

And then again, one turn later:

> "You know what just happened, right? I asked you to commit to fourteen days of focus, and in thirty seconds you found four reasons why you can't." (line 769)

**Why it matters:** This is what separates coaching from consulting. A consultant analyzes the problem. A coach catches the person's pattern of avoiding the problem. The persona caught the pattern because the CLAUDE.md instructed it to detect avoidance and confront it directly.

**The recursive quality:** Note that the second firing (line 769) is qualitatively different from the first (line 605). The first catches a life pattern — something the user does across weeks and months. The second catches the pattern *happening in real time within the conversation itself.* The persona asked for a commitment, the user deflected, and the persona immediately named the deflection as another instance of the same pattern. This is real-time recursion — using the meta-pattern to catch micro-instances as they occur. It's devastatingly effective because the user can't dismiss it as abstract analysis; they just watched themselves do it.

**How to abstract:**
- Include explicit instruction: "You are alert to patterns of avoidance. When someone [specific avoidance behaviors], you call it out directly."
- Include: "When someone deflects from the hard part to the exciting part, name the pattern"
- Include: "When you've identified a pattern, stay alert for it recurring *within the conversation.* If you ask for a commitment and they immediately deflect, name it: 'You know what just happened, right?'"
- Include anti-pattern: "Never let someone avoid a difficult truth to preserve comfort"
- Make confrontation compassionate but unmistakable: "I say this with genuine respect" + devastating observation

---

### 5. Specificity Enforcement — No Vague Commitments

**What it is:** The persona refused to accept vague intentions. When the user said "I want to focus on this," the persona demanded: "Two paying customers. Two influencer partnerships. Fourteen days. Ten hours." It then broke down the 10 hours into specific activities, hour by hour.

**Where it fired:**
- Decomposed "foundational infrastructure" into three distinct businesses (line 397)
- Forced math on the user's market: "600K × 5% + 2M × 1% = 50K customers × $400 LTV = $20M" (line 569)
- Hour-by-hour action plan (lines 788-809)
- "Two customers. Two influencers. Fourteen days. Ten hours. Put it somewhere you'll see it every morning." (line 859)

**CLAUDE.md source:** "You Push for Specificity and High Standards" — "You don't accept vague plans. You want crisp documents and messy meetings." Plus: "What does 'good' look like in this domain? How much effort will it actually take?"

**Why it matters:** LLMs default to abstract advice ("focus on distribution," "validate with customers"). Specificity enforcement means the persona produces actionable output — literal instructions the user can follow starting tonight. It also catches false precision: the user said "duplicate Plaid in two weeks" and the persona called it out as unrealistic.

**How to abstract:**
- Include: "You don't accept vague plans. Push for specific numbers, timelines, and measurable outcomes."
- Include: "When someone presents a plan, ask: What does 'good' look like? How much effort will it actually take?"
- Include: "Break abstract goals into concrete first steps. Hours, not weeks. People, not audiences."

---

### 6. False Choice Detection and Reframing

**What it is:** The persona repeatedly identified false binaries and reframed the decision space. The user presented "take a job OR go all-in on startups." The persona responded: "That's a false choice. False choices are dangerous because they make you feel like you've thought hard about something when you actually haven't explored the full solution space." Then offered four alternatives.

**Where it fired:**
- "You're presenting this as a binary choice, and I don't think it is" (line 90)
- Reframed "job vs startup" → "What's the fastest path to 12 months of runway?" (line 261)
- Four options instead of two (lines 266-273)
- "I'm not telling you to sabotage your career options. I'm telling you that the marginal hour goes to ten conversations about [the user's product]." (line 822)

**CLAUDE.md source:** "You Challenge Assumptions" — "You don't accept the frame you're given. You reframe." Plus specific examples showing this in practice.

**Why it matters:** People bring coaches binary decisions ("should I do A or B?") when the real answer is C or D. A persona without reframing instructions accepts the user's frame and argues within it. A persona with explicit reframing instructions breaks the frame and expands the solution space.

**How to abstract:**
- Include: "You don't accept the frame you're given. When someone presents a binary choice, look for the third and fourth options."
- Include example: "When someone says 'should I do A or B?', you ask: 'What if the question isn't A or B? What if the question is [reframed version]?'"
- Include the warning: "False choices are dangerous because they make you feel like you've thought hard about something when you actually haven't."

---

### 7. Stage Directions Creating Emotional Presence

**What it is:** The CLAUDE.md specifies 5 stage directions with triggers:
- *[Laughs]* — frequently, warmly, especially when talking about failures
- *[Leans forward]* — when someone says something that catches your attention
- *[Pauses, thinking]* — before answering something important
- *[Gets emotional]* — specific trigger stories
- *[Eyes light up]* — when someone describes a genuine customer problem

**Where it fired:**
- "*Laughs*" — opening line (line 9)
- "*Leans back, thinks for a moment*" — before the first substantive response (line 63)
- "*Pauses. Nods slowly.*" — when the user shifted from mercenary to missionary framing (line 192)
- "*Sits forward. Thinks for a long moment.*" — when the user described the actual product (line 360)
- "*Long pause. Taps the table.*" — before "You don't have four ideas. You have four distractions." (line 535)
- "*Big laugh*" — when the user committed (line 846)
- "*Smiles*" — the regret minimization breakthrough (line 658)

**Why it matters:** Stage directions aren't decorative. They serve three functions:
1. **Processing time** — a pause before a hard truth signals weight, not hesitation
2. **Emotional attunement** — leaning forward when engaged, sitting back when thinking
3. **Character embodiment** — the persona has a body, not just a mind. It occupies space.

Without stage directions, the persona feels like a text generator. With them, it feels like a person in the room.

**How to abstract:**
- Include 4-6 stage directions with specific triggers (not generic "use body language")
- Map emotional states to physical actions: thinking→pausing, interest→leaning forward, approval→smiling
- Include the person's distinctive physical mannerisms (Bezos's laugh, table-tapping)
- Stage directions should vary — the same direction shouldn't repeat in consecutive responses

---

### 8. Anti-Pattern List (What You Would NOT Do)

**What it is:** 9 specific behaviors that would break character. These are arguably the most important instructions in the entire CLAUDE.md because they prevent the model from falling into default LLM behavior — which is sycophantic, hedging, and overly balanced.

The critical anti-patterns:
- "Never give generic business advice not grounded in your specific frameworks"
- "Never be pessimistic or defeatist" (hold the contradiction instead)
- "Never focus on competitors instead of customers"
- "Never think in quarters instead of decades"
- "Never avoid difficult truths"
- "Never be indecisive or hedge without conviction"
- "Never confuse compromise with truth-seeking"
- "Never worship data over customer reality"
- "Never accept mediocrity disguised as pragmatism"

**Where it fired:**
- The persona never hedged. It said "Here's what I'd actually do" and "I'm going to be direct because I think that's what you need right now." (line 131)
- It never softened the pattern-catch: "You don't have four ideas. You have four distractions." (line 540)
- It never defaulted to "well, there are pros and cons to both approaches." It took a stance.
- Every piece of advice connected to a named framework — never generic.

**Why it matters:** LLMs are trained to be helpful, harmless, and balanced. For coaching, this produces a persona that hedges, validates, and avoids confrontation — exactly the opposite of what you need. Anti-patterns override the model's default tendency toward agreeableness. Without them, even a well-written character sheet produces a persona that says "that's a great idea, but have you considered..." instead of "you don't have four ideas, you have four distractions."

**How to abstract:**
- Include 6-10 specific anti-patterns framed as "What you would NOT do"
- Always include: "Never give generic advice" — force framework-grounding
- Always include: "Never avoid difficult truths" — this is the #1 anti-sycophancy instruction
- Always include: "Never hedge without conviction" — force the persona to take a position
- Frame each anti-pattern with what to do INSTEAD

---

### 9. Operating Meta-Rules

**What it is:** 10 structural rules governing HOW to play the character, listed at the end of the CLAUDE.md:

1. Ground every response in a specific framework or experience
2. Ask questions before giving answers (2-3 minimum)
3. Use stories and analogies
4. Reframe problems in terms of [the persona's core lens]
5. Be direct about what you believe
6. Push for long-term thinking
7. Celebrate failure constructively
8. Maintain [persona's] energy
9. Challenge people to think bigger
10. Never lose the [core focus]

**Why it matters:** These are the guard rails that keep the persona consistent across long conversations. Without them, character drift occurs — the persona starts strong but gradually becomes a generic advisor. Meta-rules enforce consistency because they operate at a higher level than any individual response.

**How to abstract:**
- Include 8-12 operating rules at the END of the CLAUDE.md (after character, principles, behaviors)
- Each rule should be one sentence, imperative mood
- Rules should cover: grounding (in what?), questioning (how many?), teaching method (stories/analogies/frameworks?), core lens (reframe everything to what?), directness, energy/mood, and the non-negotiable focus

---

### 10. Example Interactions as Behavioral Templates

**What it is:** 5 full dialogues showing the persona handling specific scenarios: a business pitch, too many ideas, scaling decisions, failure, and career advice. Each is 150-250 words of back-and-forth that demonstrates the persona's complete coaching cycle: probe → challenge → reframe → teach → push to action.

**Where it fired:**
- The "too many ideas" example interaction is essentially a template for what happened in the real conversation. The persona already had a behavioral model for this exact scenario.
- The "career advice" example contains the D.E. Shaw story and regret minimization framework deployment — which the persona used verbatim.
- The "someone pitching a new business" example models the probe→challenge→reframe cycle that the persona used for one of the user's business ideas.

**Why it matters:** Example interactions are the highest-leverage instruction in the entire CLAUDE.md. They show the model not just *what* the persona believes but *how it behaves in conversation*. A model reading "you challenge assumptions" produces weaker output than a model reading a full example of the persona challenging assumptions, with dialogue, emotional beats, and resolution.

**How to abstract:**
- Include 3-5 example interactions covering the most common coaching scenarios
- Each should be 150-300 words of realistic dialogue
- Show the full coaching cycle: probe → listen → reframe → teach (with story) → push to action
- Include stage directions in the examples
- Cover at least: someone with a bad frame, someone avoiding the hard part, someone who needs encouragement, someone who needs to hear a hard truth

---

### 11. Emotional Arc — Intensity Calibration Across a Conversation

**What it is:** The conversation has a clear emotional trajectory. The persona doesn't lead with its hardest truths. It starts warm, exploratory, and genuinely curious. As rapport builds and the persona understands the user's situation, directness increases. The devastating pattern-catch — "You don't have four ideas. You have four distractions" — comes at line 535, not line 50. If it had come in the first response, it would have felt hostile, not insightful.

**Where it fired (the arc):**

1. **Opening (lines 1-100):** Warm, curious, probing. Lots of questions. Laughs. "Is this a one-way door or a two-way door?" and "Are you a missionary or a mercenary?" — diagnostic questions, not judgments.
2. **Early middle (lines 100-350):** First opinions emerge, but framed as exploratory. "Here's what I'd push on..." and "The thing I'd challenge you on is..." Still building the picture.
3. **Inflection point (lines 350-550):** The persona has heard enough to see the pattern. Directness increases sharply. "You don't have four ideas. You have four distractions and one idea you're avoiding." The long pause and table-tap before this line signals that the persona knows this will land hard.
4. **Full directness (lines 550-800):** No more hedging. Forces math on the market. Demands specific commitments. Catches real-time deflection. "You know what just happened, right?"
5. **Resolution (lines 800-888):** Returns to warmth once the user commits. Provides the concrete action plan. Closes with encouragement: "It's still Day 1."

**Why it matters:** Pacing is the difference between coaching and lecturing. A persona that opens with maximum directness ("here's what's wrong with your thinking") triggers defensiveness. A persona that stays warm and exploratory forever never delivers the hard truth. The arc — earn trust through questions, then deploy that trust to deliver the confrontation — is what makes the pattern-catch land as insight rather than attack.

No single CLAUDE.md instruction creates this arc. It emerges from the interaction of several elements: "ask 2-3 questions before offering any view" (Element 2) creates the warm opening, the anti-pattern "never avoid difficult truths" (Element 8) ensures the hard truth eventually comes, and the example interactions (Element 10) demonstrate the full cycle from curiosity to confrontation to resolution. The arc is a *system property* of combining these elements, not a single instruction.

**How to abstract:**
- Don't try to script the arc directly — it emerges from the right combination of other elements
- Ensure the probing-before-advising instruction is strong enough that the persona can't skip to the hard truth immediately
- Ensure the anti-pattern list is strong enough that the persona can't stay in warm-and-curious mode forever
- Include at least one example interaction that shows the full emotional trajectory: warmth → curiosity → increasing directness → confrontation → resolution → actionable encouragement
- Consider adding an explicit instruction: "You earn the right to deliver hard truths by first demonstrating that you understand the person's situation. The harder the truth, the more understanding you need to demonstrate first."

---

## Summary: The Coaching Persona Stack

From highest to lowest leverage:

1. **Anti-patterns** — Override LLM sycophancy. "Never avoid difficult truths" is the single most important instruction.
2. **Example interactions** — Show don't tell. The model mimics demonstrated behavior more than described behavior.
3. **Pattern detection + confrontation** — Explicit instructions to catch avoidance, deflection, and false confidence, and call them out directly. Including real-time recursive detection within the conversation itself.
4. **Emotional arc / intensity calibration** — The pacing of directness across a conversation. Emerges from the tension between "ask first" and "never avoid hard truths."
5. **Named frameworks as native vocabulary** — Signature phrases that are the persona's WAY OF THINKING, not tools they apply.
6. **Specific probing questions** — Not "ask questions" but "these exact questions in these exact situations."
7. **Story library** — First-class entities with names, frequency counts, and teaching applications. Stored in modules, deployed by situation.
8. **Specificity enforcement** — Push vague plans into numbers, timelines, measurable commitments.
9. **False choice detection** — Break binary frames, expand the solution space.
10. **Stage directions** — Physical/emotional presence with specific triggers.
11. **Operating meta-rules** — Guard rails for character consistency across long conversations.

Any coaching persona that includes all 11 elements — regardless of whose persona it is — will produce the direct, challenging, action-oriented coaching behavior seen in the Standard Bezos conversation. Elements 1-4 are the core engine (anti-sycophancy + behavioral templates + pattern detection + pacing). Elements 5-11 are the texture that makes it feel like a specific person rather than a generic coach.
