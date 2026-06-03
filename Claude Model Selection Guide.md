### Which model, which plan, and how hard to think — for every task

> **Last verified:** June 2026 | Covers Free, Pro, Max, Team, Enterprise, and API users

---

## Table of Contents

1. [The Model Lineup at a Glance](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#1-the-model-lineup-at-a-glance)
2. [Plan Access — Who Gets What](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#2-plan-access--who-gets-what)
3. [The Big Decision: Which Model?](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#3-the-big-decision-which-model)
4. [Extended Thinking — What It Is and When to Use It](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#4-extended-thinking--what-it-is-and-when-to-use-it)
5. [Effort Levels: Low → Max (and Why Max Is Often Wrong)](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#5-effort-levels-low--max-and-why-max-is-often-wrong)
6. [Task-by-Task Recommendations](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#6-task-by-task-recommendations)
7. [The 60-Second Decision Flowchart](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#7-the-60-second-decision-flowchart)
8. [Context Window — Why It Matters](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#8-context-window--why-it-matters)
9. [Model Strengths Side-by-Side](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#9-model-strengths-side-by-side)
10. [Power-User Tips](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#10-power-user-tips)
11. [FAQ](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#11-faq)
12. [Glossary](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#12-glossary)

---

## 1. The Model Lineup at a Glance

Anthropic's lineup is organized in three tiers. Think of them as economy, business, and first class — same destination, different horsepower and price.

### Currently Available Models (June 2026)

|Model|Tier|Speed|Intelligence|Context|Best For|
|---|---|---|---|---|---|
|**Haiku 4.5**|Budget|⚡⚡⚡ Fastest|Good|200K|Simple, fast, high-volume|
|**Sonnet 4.6**|Balanced|⚡⚡ Fast|Excellent|1M|Default for almost everything|
|**Opus 4.6**|Premium|⚡ Moderate|Frontier|1M|Architecture, hard reasoning|
|**Opus 4.7**|Flagship|⚡ Moderate|Frontier+|1M|Agents, research, complex code|
|**Opus 4.8**|Latest|⚡ Moderate|Frontier++|1M|Most demanding tasks|

### The One-Line Summary for Each

- **Haiku 4.5** — When speed and cost matter more than depth. Your rapid-fire assistant.
- **Sonnet 4.6** — The smart default. Handles 90%+ of all real-world tasks without compromise.
- **Opus 4.6** — First Opus at Sonnet-adjacent performance. Strong for code and document work.
- **Opus 4.7** — Current flagship. New tokenizer, better agents, extended thinking at 94.2% GPQA Diamond.
- **Opus 4.8** — Newest model. Lowest rate of passing flawed code unremarked; leads on browser/computer use tasks.

> **Free users:** You get Sonnet 4.6 — not a watered-down model. The same intelligence that powers most paid interactions, with tighter daily limits.

---

## 2. Plan Access — Who Gets What

### claude.ai Plans

|Feature|Free|Pro $20/mo|Max $100/mo|Max $200/mo|
|---|---|---|---|---|
|Default model|Sonnet 4.6|Sonnet 4.6|Sonnet 4.6|Sonnet 4.6|
|Opus access|❌|✅|✅|✅|
|Extended thinking|❌|✅|✅|✅|
|Usage vs. Free|1×|~5×|~25×|~100×|
|Claude Code|❌|✅|✅ Full|✅ Full|
|Projects + Memory|✅|✅|✅|✅|
|Web search|✅|✅|✅|✅|
|Priority access|❌|✅|✅ Max|✅ Max|
|Deep Research|✅ limited|✅|✅|✅|
|Data training opt-out|❌|✅|✅|✅|

### Team & Enterprise Plans

- **Team ($30/seat/month, min 5 users):** Shared knowledge bases, admin controls, centralized billing. All Pro features plus collaboration tools.
- **Enterprise (custom, ~$60+/seat/month, min ~70 users):** Custom rate limits, SSO, advanced admin, SLA, dedicated support.

### API Plans (for Developers)

Pay per token — no subscription required. Access all models by their API string.

|Model|Input per MTok|Output per MTok|
|---|---|---|
|Haiku 4.5|$1.00|$5.00|
|Sonnet 4.6|$3.00|$15.00|
|Opus 4.6|$5.00|$25.00|
|Opus 4.7|$5.00|$25.00|
|Opus 4.7 Fast mode|$10.00|$50.00|

> **API Tip:** Extended thinking tokens are billed at the same rate as output tokens. Batch processing is 50% cheaper across all models. Prompt caching cuts repeated input costs by up to 90%.

---

## 3. The Big Decision: Which Model?

### The Rule of Thumb

> **Start with Sonnet 4.6.** Only move up if Sonnet gives you noticeably weaker results on that specific task. Only move down to Haiku if speed or cost is your primary concern.

This isn't laziness — it's efficient. Sonnet 4.6 scores 79.6% on SWE-bench (software engineering), is within 1–2 points of Opus on most benchmarks, and costs 40% less than Opus. For the vast majority of tasks, the difference is invisible.

### When to Use Each Model

#### Haiku 4.5 — Use When:

- You need an answer in under 2 seconds and accuracy doesn't need to be perfect
- You're classifying, routing, or tagging large volumes of content
- You're generating boilerplate: repetitive code, form fills, templated emails
- You're in Claude Code and the task is a simple file read, rename, or quick edit
- You're doing quick fact lookups or yes/no questions with clear answers
- Cost is a hard constraint (it's 5× cheaper than Opus on output)

**Don't use Haiku for:** nuanced reasoning, long documents, creative writing that needs polish, complex debugging, strategy, or anything where quality matters more than speed.

#### Sonnet 4.6 — Use When: (default for most users)

- Writing: blog posts, essays, emails, social media, scripts, reports
- Coding: new features, debugging, code review, refactoring, documentation
- Analysis: summarizing documents, comparing options, extracting insights
- Research: synthesizing information, answering complex questions
- Creative: stories, dialogue, brainstorming, world-building
- Data: SQL queries, Python scripts, spreadsheet formulas
- Business: strategy docs, presentations, investor updates
- This is your go-to model for 90%+ of everyday tasks

#### Opus 4.6 / 4.7 / 4.8 — Use When:

- You've tried Sonnet and the output quality genuinely isn't good enough
- You need multi-file, multi-step code refactoring across a large codebase
- You're doing scientific reasoning, graduate-level math, or research synthesis
- You're running autonomous agents that take long chains of decisions
- You need the model to self-check its own work and catch subtle errors
- You're working on architecture decisions where one wrong call costs days
- You're doing browser/computer-use tasks (Opus 4.8 leads here)
- You need the absolute highest quality on a high-stakes document

> **Opus 4.7 vs 4.8:** Opus 4.7 added a new tokenizer (up to 35% more tokens per request — benchmark your workload before migrating from 4.6). Opus 4.8 reduces the rate of passing flawed code unremarked and leads on browser/agent benchmarks. Use 4.8 for the most demanding agentic work.

---

## 4. Extended Thinking — What It Is and When to Use It

### What Is It?

Extended thinking gives Claude time to reason _before_ it answers. Instead of generating a response immediately, Claude works through the problem internally — exploring approaches, checking its own logic, backtracking when needed — then delivers a more considered answer.

You'll see a "Thinking" indicator with a timer, and an expandable "Thinking" section above the response showing a summary of Claude's reasoning process.

### How to Enable It (claude.ai)

1. Click the **model selector** and choose any **Claude 4 model** (Sonnet 4.6 or higher)
2. Click **"Search and tools"** in the lower left of the chat
3. Toggle **"Extended thinking"** on
4. Note: switching this on/off starts a new chat

> **Required plan:** Pro, Max, Team, or Enterprise. Not available on Free. **Available on:** Claude 4 models and Claude 3.7 Sonnet. **Not available on Haiku.**

### When Extended Thinking Actually Helps

Use it when the problem has meaningful depth — where there are multiple valid approaches, hidden dependencies, or where the wrong answer could cost real time/money:

- Competition-level math or logic problems
- Complex debugging where the bug isn't obvious
- Architecture decisions with many tradeoffs
- Research synthesis across many conflicting sources
- Multi-step planning with dependencies
- Scientific reasoning or proofs
- Hard coding challenges (algorithmic complexity, optimization)
- Decisions where you want to see Claude's reasoning, not just its conclusion

### When Extended Thinking Is Overkill

This is the section that can save you real quota and time:

**Do NOT use Extended Thinking for:**

- Writing a 20-line function (Sonnet without thinking handles this perfectly)
- Drafting an email or short blog post
- Simple factual questions
- Reformatting or cleaning data
- Translating text
- Summarizing a short document
- Generating a boilerplate template
- Any task where your first instinct is "this should take Claude 10 seconds"

> **The Test:** Ask yourself: "Would a smart person need to sit and think hard about this for several minutes before answering?" If the answer is no — skip extended thinking. You'll get the same result faster and preserve your usage quota.

### The Rabbit Hole Problem

When extended thinking is on and the task is simple, you're not getting a better answer — you're getting a longer path to the same answer, at higher cost and slower speed. Worse, if your Project Instruction isn't calibrated, Claude may over-engineer a solution: returning a 300-line class hierarchy for what is objectively a 20-line utility function.

The fix: be explicit. "Write a simple, minimal implementation. Do not over-engineer." is a valid and powerful instruction, especially in Projects.

---

## 5. Effort Levels: Low → Max (and Why Max Is Often Wrong)

Think of "effort" as a dial controlling how deeply Claude reasons. More effort = slower response, higher usage cost, more thorough reasoning.

### The Five Levels (Informal Scale)

|Level|When to Use|Example Prompt Signal|
|---|---|---|
|**Minimal**|Simple lookup, yes/no, format conversion|"What's the capital of France?"|
|**Low**|Short writing, basic code, standard Q&A|"Write a welcome email for my newsletter"|
|**Medium**|Most professional tasks, solid code, analysis|"Review this function and suggest improvements"|
|**High**|Complex code, multi-step reasoning, strategy|"Design a rate-limiting architecture for my API"|
|**Max**|The hardest problems; use sparingly|"Prove this algorithm is O(n log n)"|

### Effort in Claude Code (Command-Line Tool)

Claude Code recognizes thinking-depth keywords. Each level allocates more reasoning budget:

|Keyword|Effort Level|Use For|
|---|---|---|
|_(no keyword)_|Default/adaptive|Everyday tasks; Claude decides|
|`think` / `think about it`|Light|Slightly tricky questions|
|`think deeply` / `megathink`|Medium|Multi-file debugging, refactoring|
|`think harder` / `think really hard`|High|Architecture, complex algorithms|
|`ultrathink`|Maximum|The hardest problems you can throw at it|

**Example:**

```
# Wrong — wastes reasoning budget on a trivial task:
"ultrathink: rename this variable from 'x' to 'userId'"

# Right — matches effort to complexity:
"ultrathink: this async queue is causing a race condition under high load.
 Here's the full implementation. Find it and fix it."
```

### The Cardinal Rule of Effort

> **Match effort to actual complexity, not perceived importance.**

Your blog post matters to you — but it doesn't require the same cognitive load as a distributed systems problem. Reserving high-effort modes for genuinely hard problems means you get better results on those problems and faster results everywhere else.

### Adaptive Thinking (API / Sonnet 4.6 & Opus 4.6+)

The API now supports an `effort` parameter that replaces the older `budget_tokens` setting. Values: `"low"`, `"medium"`, `"high"`, `"max"`. Claude dynamically allocates reasoning based on the task when using adaptive mode. You can also steer per-message:

- Append `"Please think hard before responding."` to encourage deeper reasoning on one turn
- Append `"Answer directly without deliberating."` to suppress thinking when it's not needed

---

## 6. Task-by-Task Recommendations

### Writing & Content

|Task|Model|Extended Thinking|Notes|
|---|---|---|---|
|Tweet / LinkedIn post|Sonnet 4.6|❌ Off|Total overkill with thinking on|
|Blog post (500–2000 words)|Sonnet 4.6|❌ Off|Sonnet excels at this|
|Long-form article / essay|Sonnet 4.6|❌ Off|Add "write in my style" instructions|
|Video / podcast script|Sonnet 4.6|❌ Off|—|
|Book chapter / novel scene|Sonnet 4.6|❌ Off|Opus for very nuanced literary work|
|Email drafts|Sonnet 4.6 or Haiku|❌ Off|Haiku fine for templated emails|
|Ad copy / headlines|Sonnet 4.6|❌ Off|Try multiple variations|
|Technical documentation|Sonnet 4.6|❌ Off|—|
|Grant proposal|Opus 4.7|✅ On|High-stakes, benefits from reasoning|
|Thesis / academic paper|Opus 4.7|✅ On|Multi-step structure + argumentation|

### Coding

|Task|Model|Extended Thinking|Notes|
|---|---|---|---|
|Fix a typo / rename variable|Haiku 4.5|❌ Off|Haiku is perfect here|
|Write a 10–50 line function|Sonnet 4.6|❌ Off|No thinking needed|
|Debug an obvious error|Sonnet 4.6|❌ Off|—|
|Write unit tests|Sonnet 4.6|❌ Off|—|
|Code review & refactor|Sonnet 4.6|❌ Off|—|
|Complex algorithm design|Sonnet 4.6|✅ Optional|Thinking helps, not required|
|Multi-file refactoring|Opus 4.7|✅ On|Extended thinking catches edge cases|
|Architecture decision|Opus 4.7|✅ On|You want to see the reasoning|
|Concurrency / race condition debug|Opus 4.7/4.8|✅ On|These are genuinely hard|
|Competitive programming|Opus 4.7|✅ On|Max effort justified|
|Agentic coding sessions (Claude Code)|Opus 4.7/4.8|Auto|Claude Code routes automatically|

### Research & Analysis

|Task|Model|Extended Thinking|Notes|
|---|---|---|---|
|Simple fact lookup|Sonnet 4.6|❌ Off|Web search more useful here|
|Summarize a document|Sonnet 4.6|❌ Off|—|
|Compare two options|Sonnet 4.6|❌ Off|—|
|Literature review|Sonnet 4.6|✅ Optional|Thinking helps with synthesis|
|Competitive analysis|Sonnet 4.6|❌ Off|Pair with web search|
|Scientific paper analysis|Opus 4.7|✅ On|Graduate-level reasoning justified|
|Financial modeling|Opus 4.7|✅ On|High-stakes numeric reasoning|
|Legal document review|Opus 4.7|✅ On|Catching subtle clause interactions|
|Deep Research (multi-step)|Any + Research feature|Auto|Claude handles effort internally|

### Business & Strategy

|Task|Model|Extended Thinking|Notes|
|---|---|---|---|
|Meeting notes / summary|Sonnet 4.6|❌ Off|—|
|Slide deck outline|Sonnet 4.6|❌ Off|—|
|Job description / hiring|Sonnet 4.6|❌ Off|—|
|Business plan|Sonnet 4.6|✅ Optional|Thinking helps with consistency|
|Investor pitch|Opus 4.7|✅ On|High-stakes; reasoning visible|
|Strategic planning|Opus 4.7|✅ On|Multiple interdependent factors|
|Risk assessment|Opus 4.7|✅ On|—|

### Data & Math

|Task|Model|Extended Thinking|Notes|
|---|---|---|---|
|Simple formula / calculation|Sonnet 4.6|❌ Off|—|
|SQL query|Sonnet 4.6|❌ Off|—|
|Data cleaning script|Sonnet 4.6|❌ Off|—|
|Statistics / regression|Sonnet 4.6|✅ Optional|—|
|Complex math proof|Opus 4.7|✅ On|This is where thinking shines|
|Optimization problem|Opus 4.7|✅ On|—|
|Graduate-level physics|Opus 4.7|✅ On|94.2% GPQA Diamond in extended mode|

### Creative Projects

|Task|Model|Extended Thinking|Notes|
|---|---|---|---|
|Social media captions|Haiku 4.5 or Sonnet|❌ Off|Speed matters more here|
|Short story|Sonnet 4.6|❌ Off|—|
|Novel chapter|Sonnet 4.6|❌ Off|Consistency > depth of reasoning|
|Screenplay|Sonnet 4.6|❌ Off|—|
|Game narrative / worldbuilding|Sonnet 4.6|❌ Off|—|
|Poetry|Sonnet 4.6|❌ Off|Creative tasks rarely benefit from thinking mode|
|Complex branching game design|Opus 4.7|✅ Optional|Only if tracking many interdependencies|

---

## 7. The 60-Second Decision Flowchart

```
START: What do I need to do?
│
├── Is it a simple, fast task?
│   (lookup, rename, short email, caption, quick summary)
│   → Use SONNET 4.6, no thinking. Done.
│       └── Very high volume or speed critical? → Use HAIKU 4.5
│
├── Is it a standard professional task?
│   (article, code function, analysis, presentation, report)
│   → Use SONNET 4.6, no thinking. Done.
│
├── Is it complex with multiple interdependencies?
│   (architecture, research synthesis, multi-file refactor, hard bug)
│   → Use SONNET 4.6 with Extended Thinking ON.
│       └── Sonnet still not enough? → Upgrade to OPUS 4.7 with thinking.
│
├── Is it a frontier-level hard problem?
│   (graduate-level proof, agentic coding, browser/computer use)
│   → Use OPUS 4.7 or OPUS 4.8 with Extended Thinking ON.
│
└── Is it an autonomous long-running agent workflow?
    → Use OPUS 4.8 (lowest error propagation, best agent performance).
```

---

## 8. Context Window — Why It Matters

The **context window** is the maximum amount of text Claude can hold in one conversation — your messages, files, Claude's responses, and the Project Instruction all count against it.

|Model|Context Window|
|---|---|
|Haiku 4.5|200,000 tokens (~150,000 words)|
|Sonnet 4.6|1,000,000 tokens (~750,000 words)|
|Opus 4.6 / 4.7 / 4.8|1,000,000 tokens (~750,000 words)|

### What This Means in Practice

- **Sonnet and Opus can hold an entire novel in context.** Haiku is still large but has limits with very long documents.
- When context fills up, earlier content gets pushed out. Claude may start "forgetting" things said at the beginning of a very long chat.
- **The fix:** For recurring context, use Project Instructions and uploaded files rather than repeating it in the chat.
- **Extended thinking tokens also consume context.** Very long reasoning sessions on Opus 4.7's Max effort can eat into available space.

---

## 9. Model Strengths Side-by-Side

|Category|Haiku 4.5|Sonnet 4.6|Opus 4.7|
|---|---|---|---|
|Speed|★★★★★|★★★★|★★★|
|Writing quality|★★★|★★★★★|★★★★★|
|Coding (general)|★★★|★★★★★|★★★★★|
|Complex reasoning|★★|★★★★|★★★★★|
|Math / proofs|★★|★★★|★★★★★|
|Long document analysis|★★★|★★★★★|★★★★★|
|Agentic workflows|★★|★★★★|★★★★★|
|Instruction following|★★★|★★★★★|★★★★★|
|Cost efficiency|★★★★★|★★★★|★★★|
|Extended thinking support|❌|✅|✅|

---

## 10. Power-User Tips

**Tip 1: Don't leave Extended Thinking on by default** It's tempting to leave it on "just in case." Don't. It burns through Pro/Max quota faster, slows every response, and adds zero value to simple tasks. Toggle it only when you're approaching a genuinely hard problem.

**Tip 2: Add thinking instructions to your Project Instruction**

```
For simple tasks (drafting, formatting, short code), respond directly.
For complex tasks (architecture, multi-file debugging, research synthesis),
take your time and think through the problem before responding.
Never over-engineer. Prefer simple, minimal implementations unless I ask otherwise.
```

**Tip 3: Explicit simplicity beats hoping for the right complexity** For code tasks especially, if you want a minimal solution, say it:

```
Write a simple, 10-20 line implementation. No classes unless necessary.
No error handling beyond basic validation. I'll add complexity later.
```

**Tip 4: Model switching mid-conversation** You can switch models during a conversation on claude.ai using the model selector. Sonnet for most messages, then switch to Opus (with thinking on) for the one hard question in the session.

**Tip 5: Use Haiku for iteration, Sonnet for the final pass** When brainstorming or trying many variations quickly, use Haiku to generate 10 options fast. Then take the best one to Sonnet for polish.

**Tip 6: For Claude Code, match keywords to actual complexity** Don't reflexively type `ultrathink`. Measure: if the task is "add a logging line," that's a Haiku-level task. If it's "diagnose why this distributed system occasionally deadlocks under load," that's genuinely ultrathink territory.

**Tip 7: API users — set `effort` per message, not globally** In the API, steering effort per message lets you run a mixed-complexity conversation without paying max-effort rates on every turn. Most messages in a session don't need heavy reasoning.

**Tip 8: Opus 4.7 tokenizer caveat** Opus 4.7 uses a new tokenizer that can generate up to 35% more tokens for the same input compared to Opus 4.6. Per-token prices are unchanged, but your effective cost per request can increase. Benchmark before migrating in production.

**Tip 9: Free users — Sonnet is genuinely good** The free tier is not a demo. Sonnet 4.6 handles the vast majority of real-world work. The primary limitation is daily message volume, not intelligence. If you're hitting limits constantly, Pro is worth it. If you're not, the free tier is a serious tool.

**Tip 10: Extended thinking shows you Claude's reasoning** Even if you're not sure whether thinking helps, enabling it and reading the expandable "Thinking" section teaches you a lot about how Claude approaches problems — and helps you write better prompts.

---

## 11. FAQ

**Q: I'm a free user. Am I getting a worse model than paid users?** A: No. Free users get Claude Sonnet 4.6 — the same model that powers most paid interactions. You get the same intelligence, with lower daily usage limits and no access to Opus or Extended Thinking.

**Q: Is Opus always better than Sonnet?** A: On benchmarks, yes — but on most real-world tasks, the difference is undetectable. Sonnet 4.6 scores 79.6% on SWE-bench (software engineering); Opus 4.7 scores higher. For a typical blog post, email, or code function, both produce essentially identical output. Opus earns its price on the hardest 5–10% of tasks.

**Q: What's the difference between Opus 4.7 and 4.8?** A: Opus 4.8 is the newest model. It's measurably less likely to pass flawed code without flagging it, and leads on browser/computer-use agent benchmarks. For most tasks, 4.7 and 4.8 are comparable; 4.8 is the better choice for agentic workflows and tasks where catching your own errors is critical.

**Q: Will Extended Thinking always give a better answer?** A: Not for simple tasks. Extended thinking adds value proportional to task complexity. For hard reasoning problems, the improvement is dramatic. For a 20-line function, you'll wait longer and get the same (or occasionally worse, over-engineered) answer.

**Q: Can I use Extended Thinking with web search at the same time?** A: Extended thinking and web search have some compatibility restrictions — check the current state in Search and tools before combining features. For most research tasks, Deep Research (a separate feature) integrates both automatically.

**Q: Can I use Extended Thinking on the Free plan?** A: No. Extended Thinking requires a Pro, Max, Team, or Enterprise subscription on claude.ai. API users can use it on a pay-per-token basis without a subscription.

**Q: Extended Thinking was on and now it wants to start a new chat. Why?** A: Toggling Extended Thinking on/off requires a fresh conversation because it changes how the model is initialized. Your previous chat is not deleted — just close the new chat and go back to it.

**Q: I'm a developer — when should I use Haiku via the API?** A: Haiku is ideal for classification, routing, extraction from templates, moderation, and any high-volume task where you're making thousands of calls per day. At $1/$5 per MTok, it's the lowest-cost path to useful output.

**Q: What's "adaptive thinking" vs "extended thinking"?** A: Extended thinking is a mode you toggle on/off. Adaptive thinking (API only, Sonnet 4.6 and Opus 4.6+) means Claude dynamically decides how much to think based on task complexity, using an `effort` parameter. Adaptive thinking is the newer, more nuanced approach. On claude.ai, the Extended Thinking toggle is the user-facing version.

**Q: Does more thinking always mean slower responses?** A: Yes. Thinking budget scales response time roughly linearly. A "Max" effort response may take 30–60 seconds on a hard problem. For simple tasks, this is a pure waste. Reserve it accordingly.

**Q: Does Haiku support Extended Thinking?** A: No. Extended Thinking is only available on Claude 4 models (Sonnet 4.6 and above) and Claude 3.7 Sonnet.

**Q: What's Claude Mythos Preview?** A: Claude Mythos is Anthropic's most advanced frontier model, currently not available to the public due to cybersecurity concerns. It's being used by a small number of trusted organizations as part of Anthropic's Project Glasswing. See anthropic.com/glasswing for details.

---

## 12. Glossary

**Context Window** — The maximum text Claude can process in one session (input + output combined). Sonnet and Opus: 1M tokens. Haiku: 200K tokens.

**Extended Thinking** — A mode where Claude reasons internally before responding. Available on Pro+ plans, Claude 4 models only. Shown as a "Thinking" section above the response.

**Adaptive Thinking** — API feature (Sonnet 4.6 / Opus 4.6+) where Claude dynamically calibrates reasoning depth using an `effort` parameter (low / medium / high / max).

**Token** — The unit Claude processes text in. Roughly 1 token ≈ 0.75 words. Models are priced per million tokens (MTok).

**MTok** — Million tokens. The unit API pricing is quoted in.

**SWE-bench Verified** — A standard benchmark measuring how well a model resolves real GitHub software engineering issues. Higher = better coding performance. Sonnet 4.6: 79.6%. Opus 4.7: ~87.6%.

**GPQA Diamond** — A research-level reasoning benchmark testing PhD-level science. Higher = stronger reasoning. Opus 4.7 scores 94.2% with extended thinking.

**Effort Level** — How deeply Claude reasons before responding. More effort = slower, more thorough. Not all tasks benefit from high effort.

**Ultrathink** — A Claude Code keyword that allocates maximum reasoning budget. Appropriate for the hardest problems; wasteful for simple tasks.

**Budget Tokens** — Older API parameter controlling how many tokens Claude could use for thinking. Now largely replaced by the `effort` parameter in adaptive thinking mode.

**Agentic Workflow** — A task where Claude takes a sequence of autonomous actions (web searches, code execution, file operations) to complete a multi-step goal, rather than responding in a single turn.

**Prompt Caching** — API feature that stores repeated inputs (like long system prompts or documents) so they don't need to be re-processed each request. Cuts repeated input costs by up to 90%.

**Batch Processing** — API feature for non-urgent, high-volume requests processed asynchronously. 50% cheaper across all models.

---

_Last updated: June 2026_ _Benchmarks and pricing verified from official Anthropic documentation and public sources._ _For the most current model strings, see: [docs.claude.com](https://docs.claude.com/)_ _For plan pricing and limits: [support.claude.com](https://support.claude.com/)_