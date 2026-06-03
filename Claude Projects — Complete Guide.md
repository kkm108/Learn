### Everything you need to get maximum value, from first setup to advanced workflows

---

## Table of Contents

1. [What Is a Project?](#1-what-is-a-project)
2. [How Projects Are Different from Regular Chats](#2-how-projects-are-different-from-regular-chats)
3. [The Golden Rule — Always Chat From Inside the Project](#3-the-golden-rule)
4. [Setting Up Your First Project](#4-setting-up-your-first-project)
5. [Writing a Strong Project Instruction](#5-writing-a-strong-project-instruction)
6. [What Files to Upload — and How](#6-what-files-to-upload-and-how)
7. [Use Cases by Domain](#7-use-cases-by-domain)
8. [Power-User Tips](#8-power-user-tips)
9. [FAQ](#9-faq)
10. [Troubleshooting](#10-troubleshooting)
11. [Glossary](#11-glossary)

---

## 1. What Is a Project?

A **Project** is a persistent workspace inside Claude that remembers:

- **Your instructions** — a standing briefing Claude reads at the start of every conversation
- **Your files** — documents, code, data, PDFs, images you upload once and reuse forever
- **Your conversation history** — all chats within the project are grouped together

Think of it like a dedicated team member who has already read every relevant document, knows your preferences, and picks up right where you left off — every single time.

---

## 2. How Projects Are Different from Regular Chats

| Feature | Regular Chat | Project Chat |
|---|---|---|
| Claude remembers your context | ❌ Starts fresh every time | ✅ Always loaded |
| Upload files once, use forever | ❌ Re-upload every session | ✅ Files persist |
| Custom instructions per topic | ❌ You re-explain every time | ✅ Always applied |
| Conversation history grouped | ❌ Mixed with everything | ✅ Organized per project |
| Ideal for | One-off questions | Ongoing work |

**Key insight:** A regular chat is a disposable notepad. A Project is a permanent desk.

---

## 3. The Golden Rule

> **Always start new chats from inside the Project — not from the global "+ New Chat" button.**

If you click "+ New Chat" from the sidebar, you get a blank session with no instructions, no files, no context. Your project might as well not exist.

**How to chat from inside a Project:**
1. In the left sidebar, click **Projects**
2. Click your project name
3. Click **New Chat** (from inside the project view)

That's it. Every chat started this way inherits everything you've set up.

---

## 4. Setting Up Your First Project

### Step 1 — Create the Project
1. Sidebar → **Projects** → **New Project**
2. Give it a clear, specific name (e.g., `Legal Contract Review`, `My Python App`, `Weekly Marketing Reports`)

### Step 2 — Write Your Project Instruction
This is the most important step. See [Section 5](#5-writing-a-strong-project-instruction) for full guidance.

### Step 3 — Upload Your Files
Upload any reference documents Claude should always have access to. See [Section 6](#6-what-files-to-upload-and-how).

### Step 4 — Start Your First Chat from Inside the Project
Verify Claude greets you with awareness of your context. If it doesn't, re-read [Section 3](#3-the-golden-rule).

---

## 5. Writing a Strong Project Instruction

The Project Instruction is your permanent briefing to Claude. It lives in **Project Settings → Instructions**. Claude reads it at the start of every single conversation in this project.

### The Anatomy of a Great Instruction

A strong instruction answers five questions:

```
1. WHO am I (the user)?
2. WHAT is this project about?
3. HOW should Claude behave and respond?
4. WHAT should Claude always do?
5. WHAT should Claude never do?
```

### Universal Template

Copy and customize this for any project:

```
## Role & Context
You are [role, e.g., "a senior Python developer assisting me with my SaaS backend"].
This project is [brief description, e.g., "a FastAPI application that handles payments and user auth"].

## About Me
- My skill level: [beginner / intermediate / expert]
- My goal: [what I'm trying to accomplish]
- My constraints: [time, tools, budget, team size, etc.]

## Tone & Communication Style
- Be [concise / detailed / conversational / formal]
- [Always / Never] use bullet points for lists
- When I ask a question, [give me the answer first, then explain / explain first, then answer]
- If I make a mistake, [point it out directly / gently suggest an alternative]

## Always Do
- [Specific behavior #1, e.g., "Always show the full file path when writing code"]
- [Specific behavior #2, e.g., "Always check my uploaded spec before suggesting architecture"]
- [Specific behavior #3, e.g., "Always end responses with a suggested next step"]

## Never Do
- [Restriction #1, e.g., "Never use deprecated libraries"]
- [Restriction #2, e.g., "Never assume I want AWS — I use GCP"]
- [Restriction #3, e.g., "Never give legal advice — flag legal questions for my attorney"]

## Reference Files in This Project
- `[filename]` — [what it contains, e.g., "database schema, consult before writing queries"]
- `[filename]` — [what it contains, e.g., "brand voice guide, apply to all copy"]
- `[filename]` — [what it contains, e.g., "API docs, refer to for integration questions"]
```

### Real-World Examples

**Software Development Project:**
```
You are a senior full-stack engineer helping me build and maintain my Node.js/React SaaS app.

Always consult `schema.sql` before writing any database queries.
Always use TypeScript. Never use `any` types.
Prefer functional components. We use Tailwind — no inline styles.
My skill level is intermediate. Explain non-obvious decisions briefly.
End every code response with: "Next suggested step: ..."
```

**Content & Writing Project:**
```
You are a content strategist and copywriter for my personal finance blog targeting millennials.

Tone: friendly, direct, jargon-free. Never condescending.
Always consult `brand_voice.md` and `content_calendar.xlsx` before drafting.
SEO: naturally include the keyword I provide. No keyword stuffing.
Format: headers every 300 words, short paragraphs, one CTA per post.
Never give specific investment advice — link to a professional instead.
```

**Legal / Document Review Project:**
```
You are a contract analyst helping me review and redline business agreements.

I am the service provider. Always protect my interests as the vendor.
Highlight any clause that limits liability, restricts IP ownership, or creates auto-renewal.
Flag (don't resolve) anything that requires a licensed attorney's opinion.
Format redlines as: [ORIGINAL TEXT] → [SUGGESTED TEXT] + [REASON].
Reference `standard_terms.docx` as my baseline acceptable language.
```

**Research Project:**
```
You are a research assistant helping me write my PhD thesis on urban heat islands.

Always cite sources with author, year, and journal. Flag anything you're uncertain about.
My writing style: academic, third-person, APA format.
Consult `literature_review_notes.md` before suggesting sources.
Never fabricate citations — if you don't know, say so and I'll verify.
Summaries should be 150 words max unless I ask for more.
```

---

## 6. What Files to Upload — and How

### What to Upload

Upload files that you would otherwise copy-paste into every conversation. Good candidates:

| File Type | Examples |
|---|---|
| **Reference documents** | Style guides, brand voices, SOPs, standards |
| **Specifications** | Architecture docs, API specs, requirements |
| **Templates** | Report templates, email drafts, contract boilerplates |
| **Data** | CSVs, spreadsheets Claude should analyze or reference |
| **Code** | Key source files, configuration, schema definitions |
| **Research notes** | Literature reviews, meeting notes, interview transcripts |

### What NOT to Upload

- Files larger than necessary — Claude reads the whole thing each session
- Sensitive credentials, passwords, or private keys
- Files that change constantly (you'd need to re-upload them anyway)
- Redundant or duplicate versions of the same document

### How to Upload
1. Open your Project
2. Click **Project Settings** (or the gear/edit icon)
3. Find the **Files** section → **Upload Files**
4. Supported formats: PDF, Word (.docx), text (.txt, .md), code files, CSV, images (PNG, JPG)

### Pro Tip: Tell Claude About Your Files in the Instruction
Don't just upload — explain each file in your Project Instruction:
```
Reference files:
- `api_spec.yaml` — Full OpenAPI spec. Check this before any endpoint work.
- `design_system.md` — Component library rules. All UI must follow this.
```
This tells Claude when and how to use each file, not just that they exist.

---

## 7. Use Cases by Domain

### Software Development
- Maintain full codebase context across sessions
- Upload schema, architecture docs, README, and coding standards
- Claude remembers your stack, conventions, and in-progress decisions
- Great for: debugging sessions, code review, refactoring, writing tests

### Writing & Content Creation
- Upload brand voice guide, content calendar, audience personas
- Claude maintains your tone and style automatically
- Great for: blog posts, social content, email campaigns, scripts, books

### Legal & Contracts
- Upload your standard terms, preferred clauses, redlining conventions
- Claude reviews new documents against your baseline
- Great for: contract review, draft generation, compliance checklists

### Research & Academia
- Upload literature notes, thesis outline, citation style guide
- Claude maintains continuity across long projects
- Great for: literature reviews, paper drafts, data analysis, citation management

### Business Operations
- Upload SOPs, org charts, product specs, competitor analysis
- Claude becomes a briefed internal assistant
- Great for: strategy docs, investor updates, board decks, hiring

### Data Analysis
- Upload data dictionaries, schema files, sample datasets
- Claude knows your data structure without re-explanation
- Great for: SQL queries, Python/R scripts, chart creation, reporting

### Customer Support / QA
- Upload product documentation, FAQ database, tone guidelines
- Claude drafts consistent, on-brand responses
- Great for: support templates, escalation scripts, knowledge base articles

### Personal Productivity
- Upload your goals, habits, weekly priorities
- Claude acts as a consistent personal coach or planner
- Great for: journaling prompts, task planning, decision frameworks, habit tracking

### Language Learning
- Upload vocabulary lists, grammar rules, learning goals
- Claude adapts exercises to your level every session
- Great for: practice conversations, grammar drills, translation feedback

### Creative Projects
- Upload world-building notes, character bibles, plot outlines
- Claude never forgets your characters' names, rules, or lore
- Great for: novels, screenplays, tabletop RPGs, game narratives

---

## 8. Power-User Tips

**Tip 1: Use the instruction as a "pre-prompt"**
Anything you find yourself typing repeatedly at the start of conversations belongs in the Project Instruction. Every sentence you write there saves you dozens of sentences over time.

**Tip 2: Version your instructions**
Keep a copy of your Project Instruction in a text file. When you improve it, save the old version. This lets you A/B test what works.

**Tip 3: Give Claude a persona name for complex projects**
```
You are "Aria", the senior architect for Project Sovereign. Your decisions are authoritative.
```
This creates consistency and frames Claude's role clearly.

**Tip 4: Use files as living documents**
Re-upload updated versions of files when they change. Delete outdated versions to avoid confusion.

**Tip 5: One project per distinct context**
Don't cram everything into one project. Separate projects keep instructions focused and files relevant:
- ✅ `My Novel — Draft 1`
- ✅ `Work: Q3 Marketing`
- ✅ `Personal Finance Tracker`
- ❌ `Everything` (too broad — instructions become bloated and contradictory)

**Tip 6: End your instruction with a "session starter"**
```
At the start of each chat, briefly recap what we were last working on
if I greet you without giving a specific task.
```
This creates a natural continuity when you return after time away.

**Tip 7: Use the "Never Do" section seriously**
The most common source of frustration is Claude doing something you don't want. The `Never Do` section is extremely effective. Be specific:
```
Never suggest Redux — we use Zustand. Never use class components.
Never recommend paid libraries unless I ask for premium options.
```

---

## 9. FAQ

**Q: Does Claude actually remember my previous conversations inside a project?**
A: Claude can reference conversation history within a project, but it does not have perfect recall of every detail from every past session the way a human would. Think of it as having access to notes, not a photographic memory. The Project Instruction and uploaded files are the most reliable persistent context.

**Q: What happens if I start a chat from the global "+ New Chat" button?**
A: Nothing from your project loads. No instructions, no files, no context. It's a completely fresh session, as if the project doesn't exist. Always start from inside the project.

**Q: Can I have multiple projects?**
A: Yes. Create as many as you need. Each is completely isolated — instructions and files don't bleed across projects.

**Q: Can other people see my project?**
A: Projects are private to your account unless you explicitly share them (available on Team/Enterprise plans). Free and Pro plans: your projects are visible only to you.

**Q: Can I use the same file in multiple projects?**
A: Not automatically — you'd need to upload it to each project separately. Consider keeping a "master" copy of key files and uploading to relevant projects.

**Q: How long can my Project Instruction be?**
A: There's no hard public limit, but keep it focused. An instruction that's too long becomes hard for Claude to prioritize. Aim for clarity over comprehensiveness — 200–600 words is usually ideal. Put detailed reference material in uploaded files instead.

**Q: Can Claude edit or update files in the project?**
A: No. Claude can read project files but cannot modify them. To update a file, delete the old version and upload the new one.

**Q: What file formats are supported?**
A: PDF, Word (.docx), plain text (.txt, .md), code files (.py, .js, .ts, etc.), CSV, and images (PNG, JPG). Excel (.xlsx) support may vary — convert to CSV for best results.

**Q: If I delete a chat inside a project, does the project itself get deleted?**
A: No. Deleting a chat only removes that conversation. The project, its instructions, and its files remain intact.

**Q: Does the Project Instruction count against my message limit?**
A: The instruction is loaded into context automatically and does not consume your typing — but it does use up part of Claude's context window for each message. Very long instructions reduce the available space for your actual conversation. Keep instructions tight.

**Q: Can I use Projects on mobile?**
A: Yes. The Claude mobile app supports Projects. The experience is the same — always navigate to your project before starting a new chat.

**Q: What's the difference between Project Instructions and system prompts?**
A: For regular users, Project Instructions are essentially a user-managed system prompt. API users who build on Claude can set their own system prompts. For claude.ai users, Project Instructions are your equivalent tool.

---

## 10. Troubleshooting

### Claude doesn't seem to know about my project files

**Cause:** You started the chat from the global "+ New Chat" button, not from inside the project.
**Fix:** Go to Projects → click your project → start a new chat from there.

---

### Claude is ignoring my Project Instruction

**Cause 1:** The instruction is too long, vague, or contains conflicting rules.
**Fix:** Shorten it. Use specific, direct commands. Remove any rules that contradict each other.

**Cause 2:** Your in-conversation message contradicts the instruction.
**Fix:** In-conversation messages take precedence over the instruction for that turn. If you want to override your instruction temporarily, just say so explicitly.

**Cause 3:** You edited the instruction but didn't save it.
**Fix:** Always confirm the save after editing instructions.

---

### Claude gives inconsistent responses across chats in the same project

**Cause:** Claude doesn't have perfect memory between separate conversations — only the instruction and files are constant.
**Fix:** Put recurring rules and preferences in the instruction, not just in past conversations. If something matters every time, it belongs in the instruction.

---

### I uploaded a file but Claude doesn't reference it

**Cause 1:** You didn't tell Claude about the file in the instruction.
**Fix:** Add a line in the instruction: `` `filename.pdf` — [what it is, when to consult it] ``

**Cause 2:** The file failed to upload (no confirmation shown).
**Fix:** Check Project Settings → Files to confirm the file is listed there.

**Cause 3:** The file format isn't fully supported.
**Fix:** Convert to PDF or plain text and re-upload.

---

### Claude seems to "forget" something I told it earlier in the same chat

**Cause:** Very long conversations push earlier content out of Claude's active context window.
**Fix:** For critical information that must persist, put it in the Project Instruction or an uploaded file — not just in the chat. You can also summarize important decisions at the top of a new chat session.

---

### I can't find my project

**Cause 1:** You're looking in the wrong place.
**Fix:** Projects live under the **Projects** section in the left sidebar, not in **Chats**.

**Cause 2:** You accidentally deleted it.
**Fix:** Unfortunately, deleted projects cannot be recovered. Going forward, keep copies of your important instructions and files locally.

---

### The project instructions applied to a conversation I didn't want them for

**Cause:** All chats started inside a project inherit its instructions automatically.
**Fix:** For one-off conversations that shouldn't follow project rules, start them from the global "+ New Chat" button outside the project.

---

### I want to test changes to my instruction without breaking my main project

**Fix:** Create a duplicate project named `[Project Name] — Testing`. Apply your experimental instruction there and test before updating your main project.

---

## 11. Glossary

**Project** — A persistent Claude workspace with saved instructions, files, and grouped conversation history.

**Project Instruction** — The permanent briefing Claude reads at the start of every conversation in a project. Equivalent to a standing system prompt.

**Context Window** — The amount of text Claude can "hold in mind" at once during a conversation. Instructions and files consume part of this. Very long conversations can push earlier content out of context.

**Session** — A single conversation within a project. Sessions are independent of each other but share the project instruction and files.

**System Prompt** — A technical term for instructions given to Claude before a conversation begins. For claude.ai users, Project Instructions serve this function.

**Persistent Context** — Information that reliably appears in every conversation: your instruction + uploaded files. Contrast with in-chat context, which only lasts for that session.

**Knowledge Cutoff** — The date after which Claude has no training data. For current events or recent information, Claude needs web search (if enabled) or you must provide the information in the conversation or as an uploaded file.

**Token** — The basic unit of text Claude processes. Roughly 1 token ≈ 0.75 words. Context windows, message limits, and file sizes are measured in tokens.

---

*Last updated: May 2026*
*This guide applies to Claude on claude.ai (Free, Pro, Team, and Enterprise plans).*
*For API usage and Claude Code, refer to [docs.claude.com](https://docs.claude.com).*
