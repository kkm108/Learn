### The right tool for every job — from a quick question to a frontier-level problem

> **The Principle:** An F1 car buys groceries no better than a bicycle. It just costs more, takes longer to park, and burns a tank of fuel doing it. Match the model to the task — not to your anxiety about getting the best answer.

> **Last verified:** June 2026 | Covers cloud, open-source hosted, and offline/local models

---

## Table of Contents

1. [The Mental Model: Vehicles for Tasks](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#1-the-mental-model-vehicles-for-tasks)
2. [The Full Landscape at a Glance](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#2-the-full-landscape-at-a-glance)
3. [Cloud Models — Detailed Profiles](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#3-cloud-models--detailed-profiles)
4. [Specialist & Embedded AI Tools](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#4-specialist--embedded-ai-tools)
5. [Offline / Local Models](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#5-offline--local-models)
6. [Hardware Guide for Running Local Models](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#6-hardware-guide-for-running-local-models)
7. [Task-by-Task Routing Guide](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#7-task-by-task-routing-guide)
8. [Decision Flowchart: Which Model?](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#8-decision-flowchart-which-model)
9. [When to Go Local vs Cloud](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#9-when-to-go-local-vs-cloud)
10. [Cost Comparison Reference](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#10-cost-comparison-reference)
11. [Power-User Tips](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#11-power-user-tips)
12. [FAQ](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#12-faq)
13. [Glossary](https://claude.ai/chat/baeea242-fa21-4f04-a49e-ae3b1fd23db3#13-glossary)

---

## 1. The Mental Model: Vehicles for Tasks

Before picking a model, ask one question: **what level of horsepower does this actually require?**

|Vehicle|Equivalent AI Tier|When You Need It|
|---|---|---|
|🚶 Walk|No AI needed|Copy-pasting, a Google search, a spell-checker|
|🚲 Bicycle|Tiny/fast model (Haiku, Gemini Flash, Phi-4 mini)|Short Q&A, quick format, caption, lookup|
|🚗 Car|Balanced model (Sonnet, GPT-5.4, Gemini 3.1)|90% of professional daily work|
|🏎️ Sports Car|Capable frontier model (Opus, GPT-5.5)|Hard code, research synthesis, strategy|
|🚀 F1 / Rocket|Max-effort frontier + extended thinking|Competition math, doctoral reasoning, frontier agents|

**The most common mistake:** Reaching for the F1 car for every task. This wastes quota, slows responses, and often produces over-engineered answers for simple problems. The second most common mistake: using a bicycle for a problem that genuinely needs a car.

---

## 2. The Full Landscape at a Glance

### Cloud / API Models

|Model|Provider|Tier|Free Tier|Paid|Best Single Use Case|
|---|---|---|---|---|---|
|**GPT-5.5**|OpenAI|Frontier|✅ Limited|$20–200/mo|Agentic workflows, multimodal|
|**GPT-5.4 mini**|OpenAI|Balanced|✅|$20/mo|Everyday tasks, fast coding|
|**Claude Sonnet 4.6**|Anthropic|Balanced|✅|$20/mo|Writing, long documents, coding|
|**Claude Opus 4.8**|Anthropic|Frontier|❌|$20–200/mo|Agentic coding, complex reasoning|
|**Gemini 3.1 Pro**|Google|Frontier|✅ Limited|$20/mo|Reasoning, Google Workspace|
|**Gemini 3 Flash**|Google|Fast|✅ Generous|$20/mo|Quick tasks, high volume|
|**Grok 4.3**|xAI|Frontier|✅ Limited|$25–300/mo|Real-time data, raw SWE-bench coding|
|**DeepSeek V4**|DeepSeek|Frontier|✅|API only|Budget frontier, math, cost-sensitive|
|**Mistral Large 3**|Mistral|Capable|✅ Limited|$14.99/mo|EU privacy, multilingual, structured output|
|**Mistral Small 3.1**|Mistral|Balanced|✅|$14.99/mo|Fast, cheap, EU-compliant|
|**Llama 4 Maverick**|Meta (hosted)|Frontier|✅✅ Free|Free|Open-weight alternative to GPT-4o-class|
|**Qwen 3.5**|Alibaba (hosted)|Capable|✅|API|Coding, math, multilingual, CJK|

### Specialist Tools (AI-powered, not raw LLMs)

|Tool|Best For|Free Tier|
|---|---|---|
|**Perplexity**|Cited real-time research|✅ Generous|
|**Microsoft Copilot**|Microsoft 365 / Office integration|✅|
|**NotebookLM**|Summarizing & querying your own documents|✅✅|
|**GitHub Copilot**|In-IDE code completion and chat|$10–39/mo|
|**Meta AI**|Casual chat, WhatsApp/Instagram embedded|✅✅ Fully free|
|**Google AI Studio**|API prototyping, free Gemini access|✅✅|

### Local / Offline Models

|Model|Size|Min RAM|Best For|
|---|---|---|---|
|**Gemma 4 E4B**|4B|3 GB VRAM|Laptops, edge devices, phones|
|**Phi-4 Mini**|3.8B|4–5 GB|CPU-only, 8 GB RAM machines|
|**Phi-4**|14B|8–10 GB VRAM|Coding + reasoning on mid-range hardware|
|**Mistral Small 3.1**|24B|16 GB VRAM|Production server, structured output|
|**Qwen 3.5 7B**|7B|6–8 GB|Best small coding model|
|**Qwen 3.5 27B**|27B|16–20 GB|Frontier-competitive coding on single GPU|
|**Llama 4 Scout**|17B MoE|12–16 GB|10M token context, multimodal|
|**Gemma 4 31B**|31B|20 GB|Reasoning above its weight class|
|**DeepSeek R1 (32B distilled)**|32B|24 GB|Deep reasoning, math, privacy-sensitive|
|**DeepSeek V3.2 (full)**|685B|48 GB+|Cluster-scale; near-GPT-5 reasoning|

---

## 3. Cloud Models — Detailed Profiles

---

### 🟢 OpenAI — ChatGPT / GPT Models

**Access:** chat.openai.com | Free, Plus $20/mo, Pro $200/mo **API:** api.openai.com | GPT-5.4: ~$2.50/$10 per MTok; GPT-5.4 mini: ~$0.15/$0.60

#### GPT-5.5 (Current Flagship)

**Released:** April 2026. Currently the top score on the Artificial Analysis Intelligence Index overall.

**Strengths:**

- Best-in-class overall intelligence benchmark performance
- Leading at agentic multi-step workflows and computer use (native since GPT-5.4)
- Strong creative writing — preferred in blind writing evaluations
- Most mature plugin/tool ecosystem of any AI provider
- Widest multimodal support: text, image, audio, video, file analysis
- Five consumer tiers give granular price-capability control

**Weaknesses:**

- Most expensive closed model at $200/mo (Pro) for full access
- Reasoning adds latency — not ideal for real-time interactive tasks
- Custom GPTs can vary enormously in quality
- No equivalent to Claude's Project system for persistent context

**Best for:** Users who need one AI to do everything well; agentic workflows; multimodal tasks (image generation + analysis + audio); teams already in the OpenAI ecosystem.

**Don't use for:** Tasks that need EU data residency, maximum long-document context, or where cost per token is critical.

**Free tier:** GPT-5.4-class access with daily limits. Meaningfully capable.

---

#### GPT-5.4 mini (Balanced Workhorse)

The "car" tier. Fast, capable, cheap. Handles 85%+ of everyday professional tasks. At ~$0.15 per MTok input, it's one of the most cost-efficient capable models available. Use this as your default when using the OpenAI ecosystem before reaching for GPT-5.5.

---

### 🔵 Anthropic — Claude Models

_(Full model breakdown in the companion Claude Model Selection Guide)_

**The one-line summary for cross-model routing:**

- **Claude Haiku 4.5** → Speed, volume, simple tasks
- **Claude Sonnet 4.6** → Default for writing, coding, analysis (the "car")
- **Claude Opus 4.7/4.8** → Hard agentic coding, frontier reasoning, long documents

**Where Claude uniquely leads:**

- Best long-form writing and prose quality (preferred in 47% of blind writing tests vs 29% for GPT-5.4)
- Largest output capacity at 64K tokens — useful for generating very long documents in one turn
- Claude Code is the most capable agentic coding tool, powering Cursor and Windsurf
- 1M token context window on Sonnet and Opus — entire novels, entire codebases in context
- Strongest instruction following and Project-based persistent context

**Best for:** Writing-heavy workflows, long document work, serious coding projects, nuanced tasks requiring careful instruction following.

---

### 🟡 Google — Gemini Models

**Access:** gemini.google.com | Free, Advanced $19.99/mo **API:** aistudio.google.com (very generous free tier) | Gemini 3.1 Pro: $2/$12 per MTok

#### Gemini 3.1 Pro (Current Flagship)

**Released:** February 2026.

**Strengths:**

- Leads GPQA Diamond reasoning at 94.3% — the highest of any model on this benchmark
- Deepest native Google Workspace integration (Gmail, Docs, Drive, Meet, Sheets)
- Strong multimodal — audio, video, image, text in one model natively
- Gemini's pricing is flat up to 200K tokens, then doubles — favorable for medium-length tasks
- Google AI Studio offers a generous free API tier, making it ideal for developers prototyping
- NotebookLM (free) is built on Gemini and is outstanding for document Q&A

**Weaknesses:**

- Context pricing doubles above 200K tokens — watch costs on very long documents
- Less refined for pure creative writing compared to Claude or GPT-5.5
- Data is processed within Google's infrastructure — check for enterprise data handling needs
- Agent workflows trail Anthropic's Claude Code on real-world benchmarks

**Best for:** Research and reasoning tasks, Google Workspace power users, data analysis, STEM-heavy work, budget-conscious API users (cheapest premium tier per token among Tier-1 providers), students.

#### Gemini 3 Flash / 2.5 Flash-Lite (Speed Tiers)

Gemini 3 Flash at $0.50/$3.00 per MTok is a strong mid-tier for high-volume tasks. Flash-Lite at even lower cost is the "bicycle" of cloud APIs — fast, cheap, good enough for classification, routing, simple Q&A.

---

### ⚫ xAI — Grok

**Access:** grok.x.ai | Free (limited), SuperGrok $25/mo, SuperGrok Heavy $300/mo **API:** ~$2/$15 per MTok

#### Grok 4.3 (Current)

**Strengths:**

- Native real-time access to X (Twitter) data — unique advantage for social context, trends, current events
- Leads raw SWE-bench coding scores among cloud models in some independent benchmarks
- Most budget-friendly flagship at $2 per MTok input (API)
- Strong for financial news, social sentiment, trending topics
- Fewer content restrictions than most models — useful for creative edge cases
- Fast response times comparable to leading frontier models

**Weaknesses:**

- Best features ($300/mo SuperGrok Heavy) are significantly more expensive than rivals
- Weaker for long-form writing compared to Claude or GPT-5.5
- Privacy policy tied to xAI's practices — not suitable for sensitive enterprise data
- Less mature ecosystem — fewer integrations than OpenAI or Google
- Limited context vs 1M window rivals (though improving)

**Best for:** Social media monitoring, real-time trending analysis, developers optimizing cost-per-token, anyone needing real-time X/Twitter signals, budget-sensitive API workloads.

**Don't use for:** Enterprise sensitive data, long document workflows, fine writing, HIPAA/GDPR compliance.

---

### 🔴 DeepSeek

**Access:** chat.deepseek.com (free web) | API only (no subscription plan) **API:** ~$0.14/$0.28 per MTok (V4 Flash) — by far the lowest cost at this capability tier

#### DeepSeek V4 (Open-Weight Frontier)

**Strengths:**

- Delivers roughly 90% of GPT-5.4 quality at 1/50th the price — the most disruptive value proposition in the market
- Open-weight (MIT license) — can be self-hosted for complete data privacy
- Exceptionally strong at math reasoning — gold-medal level on math olympiad benchmarks
- V4-Flash variant at $0.14/$0.28 per MTok for high-volume cost-sensitive pipelines
- 1M context window (V4-Pro)
- Self-hosted version eliminates all API cost after hardware investment

**Weaknesses:**

- The full 685B model requires cluster-scale hardware (48 GB+ VRAM) to self-host
- Privacy concern: the hosted API sends data to DeepSeek's servers (China-based)
- V4-Pro is still in preview status — finalization timeline unpublished
- Not suitable for classified, HIPAA, or sensitive data via hosted API
- Occasional output quality inconsistency vs closed frontier models

**Best for:** API-heavy applications where cost is the binding constraint; math-heavy workloads; teams willing to self-host for privacy; open-source-first organizations; developers who need near-frontier quality at budget pricing.

**Don't use (hosted version) for:** Any sensitive or regulated data. Self-host V4 for those cases.

---

### 🟠 Mistral AI

**Access:** chat.mistral.ai (Le Chat) | Free, Pro $14.99/mo **API:** Large 3: $2/MTok input | Small 3.1: $0.20/MTok input | Apache 2.0 open-weight

#### Mistral Large 3 (Capable Tier)

**Strengths:**

- Strong multilingual capabilities — notably French, German, Italian, Spanish, and other European languages
- EU-hosted servers — GDPR compliance by default, strong for European teams and legal requirements
- Apache 2.0 open-weight license on smaller models — commercially safe with no usage restrictions
- Native function-calling and structured JSON output — excellent for production API pipelines
- Competitive pricing vs OpenAI/Anthropic for equivalent capability tier
- Le Chat Pro at $14.99/mo — cheapest capable paid AI subscription available

**Weaknesses:**

- Trails frontier models (GPT-5.5, Opus 4.8) on pure reasoning benchmarks
- Less strong for creative long-form writing compared to Claude or GPT-5.5
- Smaller ecosystem and community vs OpenAI or Meta's Llama

**Best for:** European teams requiring EU data residency; multilingual applications; GDPR-sensitive workflows; production API pipelines needing structured output; budget-first developers who want open-source with a reliable hosted option.

**Don't use for:** Tasks requiring the absolute highest intelligence ceiling; image generation; agentic computer use.

#### Mistral Small 3.1 (Efficient Tier)

At $0.20/MTok input, it's one of the most cost-efficient capable models with vision support. The "bicycle" of capable models — fast, cheap, solid for structured tasks.

---

### 🟣 Meta — Llama / Meta AI

**Access:** meta.ai (web, WhatsApp, Instagram, Facebook) — **entirely free, no paid tier** **API (hosted):** Via Fireworks AI, Together AI, or self-hosted — among the cheapest at scale

#### Llama 4 Scout / Maverick (Open-Weight Frontier)

**Strengths:**

- **Completely free** — the only frontier-class model family with zero subscription cost
- Llama 4 Scout: 10M token context window — longest available anywhere (ideal for massive codebases or document sets)
- Llama 4 Maverick: beats GPT-4o on major benchmarks; first open-weight model at this capability tier
- MoE (Mixture-of-Experts) architecture — efficient inference relative to parameter count
- Full open weights — self-host for complete privacy, zero API cost after hardware
- Largest fine-tuning ecosystem — more task-specific variants than any other model family
- Embedded in Meta products: WhatsApp, Instagram, Facebook, Ray-Ban glasses

**Weaknesses:**

- Full models require significant hardware for local self-hosting (70B = 40 GB+ VRAM)
- The hosted meta.ai interface is more limited than Claude.ai or ChatGPT for professional workflows
- No native Projects, persistent memory, or file management at the consumer level
- Quality on the hardest tasks still trails closed frontier (GPT-5.5, Claude Opus 4.8)
- US-based data processing; Enterprise agreements needed for regulated data

**Best for:** Teams with self-hosting infrastructure; anyone who needs a completely free capable AI; developers building apps who want no licensing cost; casual users already in Meta's ecosystem; applications needing the longest context window available.

---

## 4. Specialist & Embedded AI Tools

These aren't raw LLMs — they're AI-powered products built for a specific job. Often they beat general models at their target task.

---

### 🔍 Perplexity AI

**Access:** perplexity.ai | Free, Pro $20/mo, Max $200/mo **What it is:** An AI-native search engine. Every answer cites the source. Combines LLM reasoning with live web retrieval.

**Unique strengths:**

- Every claim is cited, clickable, and verifiable — no hallucinated sources
- Real-time web access across every query — no knowledge cutoff for facts
- Pro tier lets you choose the underlying model (GPT-5.5, Claude, Gemini, or Perplexity's own)
- Max tier ($200/mo) is the only plan that gives multi-provider model access in one subscription
- Excellent for: current events, product research, fact-checking, literature scanning, competitive research

**When to use Perplexity over a general LLM:**

- You need to verify facts against the live web
- You want sources you can click and read
- You're doing research that needs to be traceable
- You're covering fast-moving topics (news, markets, policy)

**Don't use for:** Creative writing, code generation, long document analysis, tasks where you don't need citation.

**Free tier verdict:** Surprisingly capable for casual research. The Pro tier pays for itself quickly if you do more than 2 hours of research per week.

---

### 🏢 Microsoft Copilot

**Access:** copilot.microsoft.com | Free (GPT-4-class), Pro $20/mo, M365 Copilot $30/user/mo **What it is:** Two products sharing a brand: (1) consumer web chatbot and (2) Office-embedded AI.

**Unique strengths:**

- Microsoft 365 Copilot integrates AI directly into Word, Excel, PowerPoint, Outlook, and Teams
- Best choice if your organization is already paying for Microsoft 365 — no separate subscription
- Enterprise compliance (EU Data Boundary, SOC 2, HIPAA BAA) built in at the enterprise tier
- PowerPoint auto-generation from prompts, Excel formula assistant, email drafting in Outlook
- GitHub Copilot (separate product, $10–39/mo) is the best-value pure coding assistant

**When to use Microsoft Copilot:**

- Your team runs on Microsoft 365 and you want AI embedded in existing tools
- You want enterprise compliance out-of-the-box
- You need PowerPoint / Excel / Word AI without copy-pasting between tabs

**Don't use for:** Long-form creative writing, frontier-level reasoning, tasks requiring the latest models, privacy-first workflows.

---

### 📓 Google NotebookLM

**Access:** notebooklm.google.com | **Free** **What it is:** Upload your own documents; ask questions; NotebookLM cites answers directly from your sources.

**Unique strengths:**

- Works entirely on _your_ documents — no hallucinations from training data, only from your sources
- Up to 50 sources, 500K words per notebook
- Generates summaries, FAQs, study guides, podcast-style audio overviews from your materials
- Free, no subscription needed
- Excellent for: research papers, legal documents, textbooks, meeting transcripts, manuals

**When to use NotebookLM over a general LLM:**

- You're analyzing a specific set of documents and need answers grounded in _those exact files_
- You want source citations you can trust (it quotes exactly from your uploads)
- You're studying a large corpus of material

**Don't use for:** Tasks requiring general world knowledge, code generation, creative writing, anything not in your uploaded documents.

---

### 💻 GitHub Copilot

**Access:** github.com/features/copilot | Free (limited), Pro $10/mo, Pro+ $39/mo **What it is:** In-IDE AI coding assistant. Autocomplete, chat, code review, PR descriptions, test generation.

**Unique strengths:**

- $10/month Pro is the cheapest premium AI coding subscription available
- Native integration into VS Code, JetBrains, Neovim, Visual Studio
- Pro+ gives access to Claude Opus 4, o3, and all premium models inside the IDE
- Code completions happen inline as you type — lowest friction of any coding tool
- 300 premium requests/month on Pro; unlimited on Pro+

**When to use GitHub Copilot:**

- You write code in an IDE and want autocomplete + inline chat
- You want the cheapest premium coding AI ($10/mo vs $20/mo for Claude/ChatGPT)
- You're doing PR reviews, commit messages, test generation in a GitHub workflow

**Don't use for:** Non-coding tasks, long document analysis, research, anything outside a developer workflow.

---

### 📱 Meta AI (Consumer)

**Access:** meta.ai, WhatsApp, Instagram, Facebook, Ray-Ban smart glasses | **Completely free** **What it is:** Llama 4-powered assistant embedded across Meta's consumer products.

**Best for:** Casual questions, quick help while in WhatsApp/Instagram, zero-cost general assistance, users who don't want another subscription. **Don't use for:** Professional or sensitive workflows, anything requiring precision or document management.

---

## 5. Offline / Local Models

Local models run entirely on your own hardware. No API cost after setup, no data leaving your machine, no rate limits, and full availability without internet. The tradeoff: setup effort, hardware constraints, and quality ceiling below top closed models — though the gap has closed dramatically.

### Why Go Local?

1. **Privacy** — Data never leaves your device. Critical for HIPAA, GDPR, legal, financial, and proprietary data.
2. **Cost** — Zero ongoing cost after hardware purchase. At scale, this beats any API within months.
3. **No rate limits** — Run as many requests as your hardware allows, 24/7.
4. **Offline capability** — Works with no internet connection.
5. **Customization** — Fine-tune on your own data, modify the model, run custom prompts.

### The Tool: Ollama (Recommended for Beginners)

Ollama is the easiest way to run local models. One command installs it; one command downloads and runs any model.

```bash
# Install (Mac/Linux)
curl -fsSL https://ollama.com/install.sh | sh

# Pull and run a model
ollama run phi4          # Phi-4 14B — good for 8-16 GB machines
ollama run gemma4:4b     # Gemma 4 4B — for laptops with 4-8 GB
ollama run qwen3:7b      # Qwen 3.5 7B — best small coder
ollama run llama4:scout  # Llama 4 Scout — 10M context
```

**Alternatives:** LM Studio (GUI, beginner-friendly), Jan AI (lightweight), GPT4All (CPU-optimized).

---

### Local Model Profiles

#### Gemma 4 E4B / 4B (Google) — The Pocket Rocket

- **Parameters:** ~4B | **Min RAM:** 3 GB VRAM | **Ollama:** `ollama run gemma4:4b`
- Runs on laptops, phones, MacBooks with 8 GB RAM, even Raspberry Pi-class devices
- Multimodal: understands images and audio — unique at this size
- WebGPU support: the only top-tier model that runs directly in a browser tab
- **Best for:** Lightweight local assistant, edge devices, casual Q&A, image questions on minimal hardware
- **Not for:** Complex reasoning, long code generation, multi-step tasks

#### Phi-4 Mini / Phi-4 (Microsoft) — Punching Above Its Weight

- **Phi-4 Mini (3.8B):** 4–5 GB RAM | **Ollama:** `ollama run phi:3.8b`
- **Phi-4 (14B):** 8–10 GB VRAM | **Ollama:** `ollama run phi4`
- Microsoft's data-curation approach makes these models outperform rivals 2× their size
- Phi-4 competes with 30B models on coding and reasoning benchmarks
- MIT license; excellent documentation
- **Best for:** Developers with 8–16 GB machines who need strong coding + reasoning; CPU-only setups (Phi-4 Mini)
- **Not for:** Very long documents, multilingual, creative writing

#### Qwen 3.5 7B / 27B (Alibaba) — The Coding Champion

- **7B:** 6–8 GB VRAM | **Ollama:** `ollama run qwen3:7b`
- **27B:** 16–20 GB VRAM | **Ollama:** `ollama run qwen3:27b`
- Best small coding model at 7B class — scores highest on HumanEval among 7/8B models
- 27B scores 72.4% on SWE-bench Verified — near-frontier on a single 16 GB GPU
- Exceptional multilingual: best for CJK (Chinese, Japanese, Korean) and other non-English tasks
- MoE variants available for better inference efficiency
- **Best for:** Developers who need strong local coding; multilingual apps; teams with 16–20 GB GPUs
- **Not for:** Tasks requiring 100K+ token context on the 7B variant; creative writing

#### Mistral Small 3.1 (24B) — The Production Server Model

- **Min VRAM:** 16 GB | **Ollama:** `ollama run mistral-small3.1`
- Native function-calling and JSON mode — excellent for local API servers
- Strongest European language support of any local model
- Apache 2.0 license — commercially safe, self-hostable for production
- Fast tokens-per-second on RTX 4090 or equivalent
- **Best for:** Local production API server; structured output; EU-language apps; teams self-hosting for compliance
- **Not for:** 8 GB machines; CPU-only inference; tasks needing 70B+ quality

#### Llama 4 Scout (Meta) — The Context Monster

- **Architecture:** 17B MoE | **Min VRAM:** 12–16 GB | **Ollama:** `ollama run llama4:scout`
- **10M token context window** — the longest available anywhere, cloud or local
- First open-weight multimodal model at frontier scale (text + images)
- MoE architecture means fewer parameters are active per forward pass — more efficient than its size suggests
- Largest fine-tuning ecosystem; most community support
- **Best for:** Extremely long document analysis; multimodal local apps; teams needing massive context; applications where ecosystem breadth matters
- **Not for:** Pure coding performance (Qwen wins there); CPU-only machines

#### Gemma 4 31B (Google) — The Reasoning Overperformer

- **Min VRAM:** 20 GB | **Ollama:** `ollama run gemma4:27b`
- Outperforms models 20× its size on reasoning benchmarks
- Strong instruction following and output formatting
- **Best for:** Workstations with 24 GB cards (RTX 4090) that need strong reasoning without a massive model
- **Not for:** Laptops, 16 GB machines, coding-primary workflows

#### DeepSeek R1 Distilled 32B — The Privacy-First Reasoner

- **Min VRAM:** 24 GB | MIT license | **Ollama:** `ollama run deepseek-r1:32b`
- Distilled from the 671B DeepSeek R1 reasoning model — retains most of the reasoning quality
- Gold-medal math olympiad reasoning locally on a single RTX 4090 or M2 Max+
- Best choice when you need serious reasoning _and_ data must not leave your machine
- **Best for:** HIPAA/legal/financial data requiring deep reasoning; PhD-level math locally; privacy-first enterprise
- **Not for:** 16 GB machines; coding tasks (Qwen 3.5 is better there); real-time chat speed

---

## 6. Hardware Guide for Running Local Models

### The Tiers

|Hardware|VRAM / RAM|What Runs Well|
|---|---|---|
|**CPU-only laptop**|8 GB system RAM|Phi-4 Mini (3.8B), Gemma 4 2B, Llama 3.2 3B|
|**Entry GPU / MacBook**|8 GB VRAM|Phi-4 Mini, Qwen 3.5 7B, Mistral 7B, Gemma 4 4B|
|**Mid-range GPU**|12–16 GB VRAM|Phi-4 14B, Qwen 3.5 7B–27B, Llama 4 Scout, Gemma 3 12B|
|**High-end GPU**|24 GB VRAM (RTX 4090)|Mistral Small 3.1, Gemma 4 31B, DeepSeek R1 32B|
|**Workstation (dual GPU)**|40–48 GB VRAM|Llama 4 70B, Qwen 3.5 72B, larger DeepSeek distills|
|**Cluster**|48 GB+ multi-GPU|DeepSeek V3.2 full (685B) — not consumer hardware|

### Apple Silicon Note

Apple Silicon uses **unified memory** — the full RAM pool is available as GPU memory. An M3 Pro with 36 GB RAM effectively has 36 GB "VRAM." An M4 Max with 128 GB RAM can run massive models (the Qwen 3.5 235B MoE at Q4 quantization runs at 5.5+ tokens/second on the M4 Max).

### Speed Expectations

- **CPU only:** 2–8 tokens/second (slow but usable for batch tasks)
- **8 GB GPU, 7B model at Q4:** 20–50 tokens/second (conversational speed)
- **16 GB GPU, 13B model:** 15–30 tokens/second
- **RTX 4090, 32B model:** 8–15 tokens/second

### Quantization (Q4 vs Q8)

Quantization reduces the model's precision to save memory:

- **Q4 (4-bit):** Fits much larger models in limited RAM; minor quality loss (~5%)
- **Q8 (8-bit):** Better quality, needs more RAM
- **Rule of thumb:** Start with Q4 to confirm the model fits, then try Q8 if quality matters and you have headroom

---

## 7. Task-by-Task Routing Guide

### Writing & Content

|Task|Best Model|Tier|Notes|
|---|---|---|---|
|Tweet / social caption|Claude Sonnet, GPT-5.4 mini, or even Haiku|🚲 Bicycle|Any capable model handles this|
|Email drafts|Sonnet, GPT-5.4 mini|🚗 Car|Simple templates → Haiku or local Phi-4|
|Blog post|Claude Sonnet, GPT-5.5|🚗 Car|Claude leads on prose quality|
|Long article / essay|Claude Sonnet or Opus|🚗–🏎️|Claude's 64K output limit is an advantage|
|Novel chapter|Claude Sonnet|🚗 Car|Creative writing = Claude's strongest suit|
|SEO content at scale|Mistral Small, Haiku, local Qwen|🚲 Bicycle|Volume > quality; local models save money|
|Technical documentation|Claude Sonnet|🚗 Car|—|
|Grant proposal / thesis|Claude Opus, GPT-5.5|🚀 Rocket|High stakes justifies the horsepower|

### Research & Fact-Finding

|Task|Best Model|Notes|
|---|---|---|
|Quick fact lookup|**Perplexity (free)**|Citations, real-time, faster than a general LLM|
|Current events / news|**Perplexity** or **Grok** (X data)|Both have live web access|
|Document Q&A (your files)|**NotebookLM (free)**|Best at staying within your source material|
|Literature review|Perplexity Pro + Claude Opus|Perplexity for sources, Claude for synthesis|
|Competitive analysis|Perplexity + Gemini 3.1 Pro|Perplexity cites, Gemini reasons|
|PhD-level science|Gemini 3.1 Pro or Claude Opus + Extended Thinking|Gemini leads GPQA Diamond (94.3%)|
|Math proofs / olympiad|DeepSeek V4 or local DeepSeek R1|Gold-medal benchmark performance|

### Coding & Development

|Task|Best Model|Notes|
|---|---|---|
|In-IDE autocomplete|**GitHub Copilot Pro** ($10/mo)|Best value; native IDE integration|
|Quick 10–30 line function|Claude Sonnet, GPT-5.4 mini|No extended thinking needed|
|Debugging obvious error|Claude Sonnet|—|
|Code review|Claude Sonnet or Opus|—|
|Multi-file refactoring|Claude Opus 4.8|Best agentic coding tool available|
|Competition coding|Claude Opus + Extended Thinking|Or DeepSeek R1 locally|
|Coding on private codebase|**Local Qwen 3.5 27B** or **Claude Code**|Local = no data leaves machine|
|Quick scripts on budget|**Local Phi-4**, **Mistral Small**|Zero API cost|

### Business & Productivity

|Task|Best Model|Notes|
|---|---|---|
|Meeting notes / summary|Gemini (if Google Meet), Copilot (if Teams)|Use embedded tools, not general LLMs|
|Email in Outlook|**Microsoft Copilot**|Native integration beats copy-paste|
|Slides / PowerPoint|**Copilot** (M365) or GPT-5.5|Copilot generates slides from prompts in-app|
|Excel formulas|**Copilot** (M365) or Claude Sonnet|—|
|Strategy document|Claude Opus, GPT-5.5|High-stakes writing benefits from top models|
|Investor pitch|Claude Opus|Claude's writing quality + reasoning|

### Data & Analysis

|Task|Best Model|Notes|
|---|---|---|
|Simple SQL query|Claude Sonnet, GPT-5.4 mini|No extended thinking|
|Complex query optimization|Claude Opus + Extended Thinking|—|
|Data cleaning Python|Claude Sonnet, local Qwen|—|
|Statistical analysis|Gemini 3.1 Pro|Reasoning + Google Sheets integration|
|Graduate-level math|DeepSeek V4, Claude Opus + Extended Thinking|DeepSeek leads math benchmarks|
|Spreadsheet analysis|**Copilot** (Excel-native) or Claude|Copilot works inside Excel|

### Privacy-Sensitive Tasks

|Scenario|Solution|
|---|---|
|Medical / HIPAA data|Self-hosted Llama 4, Qwen 3.5, or DeepSeek R1 locally|
|Legal documents (client data)|Local DeepSeek R1 32B or Mistral (EU-hosted)|
|Financial / proprietary code|Local Qwen 3.5 27B or Llama 4 Scout|
|GDPR compliance (EU)|**Mistral Le Chat** (EU-hosted) or local model|
|Classified / government|Local models only — air-gapped if required|

---

## 8. Decision Flowchart: Which Model?

```
STEP 1: Does this task involve sensitive, private, or regulated data?
│
├── YES → Does it require serious reasoning or complex code?
│   ├── YES → Local DeepSeek R1 32B or Qwen 3.5 27B
│   └── NO → Local Phi-4, Qwen 3.5 7B, or Gemma 4
│
└── NO → Continue to Step 2
│
STEP 2: Is this primarily a research / fact-finding task?
│
├── YES → Is real-time web retrieval needed?
│   ├── YES → Perplexity (citations) or Grok (X/Twitter context)
│   └── NO → NotebookLM (your docs) or Claude + web search
│
└── NO → Continue to Step 3
│
STEP 3: Are you in an existing ecosystem?
│
├── Microsoft 365 (Office/Outlook/Teams) → Copilot
├── Google Workspace (Gmail/Docs/Drive) → Gemini
├── GitHub / VS Code / JetBrains → GitHub Copilot
└── No ecosystem lock → Continue to Step 4
│
STEP 4: What's the complexity level?
│
├── Simple (lookup, caption, email, short code) → Sonnet / GPT-5.4 mini / Haiku
│   └── Very high volume or cost-critical? → Haiku / Gemini Flash / local model
│
├── Standard professional work → Claude Sonnet or GPT-5.4 (default tier)
│   └── EU data residency required? → Mistral Le Chat Pro
│
├── Hard (architecture, complex code, research synthesis)
│   → Claude Opus + Extended Thinking or GPT-5.5
│   └── Budget-conscious? → DeepSeek V4 (90% quality at ~1/50th cost)
│
└── Frontier (PhD reasoning, agentic multi-step, max intelligence)
    → Claude Opus 4.8 + Extended Thinking
    └── Or Gemini 3.1 Pro for reasoning benchmarks / STEM
```

---

## 9. When to Go Local vs Cloud

|Factor|Cloud Wins|Local Wins|
|---|---|---|
|**Setup friction**|✅ Zero setup|❌ Hours of setup|
|**Quality ceiling**|✅ Frontier models|❌ 5–15 pts below top closed models|
|**Cost at scale**|❌ Ongoing per-token cost|✅ Zero marginal cost|
|**Privacy**|❌ Data leaves your machine|✅ Data never leaves your machine|
|**Offline use**|❌ Requires internet|✅ Works without internet|
|**Rate limits**|❌ Per-plan message limits|✅ Limited only by your hardware|
|**HIPAA / GDPR compliance**|❌ Requires enterprise contracts|✅ Local = simpler compliance|
|**Customization**|❌ Fixed behavior|✅ Fine-tune on your data|
|**Updates**|✅ Automatic|❌ Manual re-download|
|**Mobile / low-RAM**|✅ Works on any device|❌ Requires capable hardware|

### The Hybrid Strategy (Power-User Move)

Route by task, not by ideology:

```
Simple / non-sensitive tasks → Cloud (Sonnet, GPT mini, Gemini Flash)
Complex / non-sensitive tasks → Cloud (Opus, GPT-5.5)
Simple / sensitive tasks → Local (Phi-4, Qwen 3.5 7B)
Complex / sensitive tasks → Local (Qwen 3.5 27B, DeepSeek R1 32B)
```

Over 40% of enterprise AI workloads in 2026 already include a local inference component for exactly this reason.

---

## 10. Cost Comparison Reference

### Consumer Subscriptions

|Service|Free|Basic Paid|Premium|
|---|---|---|---|
|**Claude** (Anthropic)|✅ Sonnet 4.6|$20/mo (Pro)|$100–200/mo (Max)|
|**ChatGPT** (OpenAI)|✅ GPT-5.4-class|$20/mo (Plus)|$200/mo (Pro)|
|**Gemini** (Google)|✅ Very generous|$19.99/mo (Advanced)|Enterprise|
|**Grok** (xAI)|✅ Limited|$25/mo (SuperGrok)|$300/mo (Heavy)|
|**Perplexity**|✅ Good|$20/mo (Pro)|$200/mo (Max)|
|**Copilot** (Microsoft)|✅ GPT-4-class|$20/mo (Pro)|$30/user (M365)|
|**Mistral Le Chat**|✅|$14.99/mo (Pro)|—|
|**Meta AI**|✅✅ Fully free|—|—|
|**GitHub Copilot**|✅ Limited|$10/mo (Pro)|$39/mo (Pro+)|
|**NotebookLM**|✅✅ Fully free|—|—|
|**Local models (Ollama)**|✅✅ Free forever|—|Hardware only|

### API Pricing (Per Million Tokens — Input / Output)

|Model|Input|Output|Use Case Tier|
|---|---|---|---|
|Gemini 2.5 Flash-Lite|$0.02|$0.08|High-volume budget|
|DeepSeek V4 Flash|$0.14|$0.28|Budget frontier|
|Mistral Small 3.1|$0.20|$0.60|Cheap + capable|
|Gemini 3 Flash|$0.50|$3.00|Mid-tier budget|
|GPT-5.4 mini|~$0.15|~$0.60|Mid-tier OpenAI|
|Grok 4.3|$2.00|$15.00|Frontier budget|
|Mistral Large 3|$2.00|$6.00|Capable mid-tier|
|Gemini 3.1 Pro|$2.00|$12.00|Premium reasoning|
|Claude Sonnet 4.6|$3.00|$15.00|Premium balanced|
|Claude Opus 4.7|$5.00|$25.00|Premium frontier|
|GPT-5.5|~$7.00|~$35.00|Premium frontier|

**Key ratio:** DeepSeek V4 Flash delivers ~90% of GPT-5.4 quality at roughly 1/50th the price. For API-heavy applications, routing simple tasks to DeepSeek Flash and complex tasks to Sonnet or GPT-5.4 is the highest-ROI optimization available.

---

## 11. Power-User Tips

**Tip 1: Build a personal model routing table** Write down the 5–10 tasks you do most often and assign a model to each. Stop re-deciding every session. Your routing table might look like:

- Morning email → Copilot (in Outlook)
- Research → Perplexity
- Blog writing → Claude Sonnet
- Private code work → Local Qwen 3.5 27B
- Hard architecture problem → Claude Opus + Extended Thinking

**Tip 2: The free-tier triangle** For many users, three free tools cover everything:

- **Claude free** (Sonnet 4.6) → writing, coding, analysis
- **Perplexity free** → research with citations
- **NotebookLM free** → your own document Q&A Total cost: $0.

**Tip 3: Local models for iteration, cloud for finals** When brainstorming or generating 10 variations, run them locally (zero cost, no rate limits). Take the best result to a cloud model for refinement and final polish.

**Tip 4: Don't equate subscription price with task quality** A $200/mo plan doesn't automatically give better results on a simple email. The model's strengths matter more than the plan tier for most tasks. Gemini Flash handles classification better than Opus 4.8 — and it's cheaper than free at API prices.

**Tip 5: DeepSeek for math, Claude for writing, Perplexity for research** These three don't overlap. Using the leader in each domain costs less and produces better results than using one model for everything.

**Tip 6: The "bicycle test"** Before opening any frontier model, ask: "Could this be answered with a Google search?" If yes — use Google. If it needs AI reasoning but is simple: use any capable model, no extended thinking. Only invoke frontier models + extended thinking when the task genuinely cannot be done any other way.

**Tip 7: Ollama is one command away** If you haven't tried a local model and have a Mac with 16 GB RAM, you're leaving a free, private, unlimited AI tool on the table. `brew install ollama && ollama run qwen3:7b` takes 5 minutes.

**Tip 8: Model routing via API (for developers)** In production, build a lightweight classifier that routes incoming requests:

- Simple/short → Haiku, Gemini Flash, or DeepSeek Flash ($0.02–0.14/MTok)
- Medium → Sonnet or GPT-5.4 ($3/MTok)
- Hard → Opus or GPT-5.5 (only when needed) This alone can reduce API costs by 60–80% with negligible quality loss on simple tasks.

---

## 12. FAQ

**Q: Should I pay for multiple AI subscriptions?** A: Most people don't need to. Pick the one that covers your primary use case (Claude for writing/coding, Gemini for Google Workspace users, ChatGPT for multimodal/agents). Add Perplexity free for research. Use NotebookLM free for document Q&A. That's typically enough.

**Q: Is there one model that's best at everything?** A: No — and this is now definitively true. GPT-5.5 leads the overall Intelligence Index. Gemini 3.1 Pro leads reasoning benchmarks. Claude leads writing quality and agentic coding. Grok leads for real-time social data. DeepSeek leads cost-efficiency. The right answer is routing, not loyalty.

**Q: Are open-source / local models good enough for real work?** A: For most professional tasks, yes. Qwen 3.5 27B on a 16 GB GPU is competitive with GPT-4o on coding and analysis tasks. The gap is most noticeable in creative writing and the very hardest reasoning problems. For sensitive data workflows, local models aren't just "good enough" — they're the right choice.

**Q: Is Perplexity the same as a search engine?** A: It synthesizes information from live web results and cites them — closer to a researched answer than a list of links. It's better than Google for questions that need synthesis; Google is better for finding specific URLs or products to browse.

**Q: What's the cheapest way to access frontier intelligence?** A: DeepSeek V4 via API ($0.14/$0.28 per MTok) delivers roughly 90% of GPT-5.4 quality. For consumer use, Claude free and Gemini free both provide frontier-adjacent quality at zero cost.

**Q: Can I use local models for production applications?** A: Yes — over 40% of enterprise AI workloads already include a local inference component. Mistral Small 3.1 and Qwen 3.5 27B are production-stable options under Apache 2.0 licenses. Self-hosting requires engineering effort but provides complete data control.

**Q: Which AI is best for non-English languages?** A: Qwen 3.5 for CJK (Chinese, Japanese, Korean) and Asian languages. Mistral for European languages (especially French, German, Italian, Spanish) with EU data residency. Gemini has broad multilingual support. GPT-5.5 is capable but not the specialist in any language.

**Q: Does using a more expensive model always give better results?** A: No. For simple tasks, premium models don't produce better output — just slower and more expensive output. On a 20-line function or a brief email, Haiku, Gemini Flash, and Phi-4 produce identical results to Opus 4.8 or GPT-5.5. Save the expensive models for tasks that genuinely need them.

**Q: What about image generation?** A: This guide covers language models. For image generation: DALL-E 3 (via ChatGPT), Midjourney, Stable Diffusion (local, open-source), Ideogram, and Adobe Firefly are the leading tools. Gemini natively handles image understanding but delegates generation to Imagen.

**Q: Is it safe to send my code to cloud AI models?** A: Review each provider's data use policy. For proprietary or client code: Anthropic Pro/Team plans and OpenAI Team/Enterprise allow opting out of training data collection. For maximum safety: use local models (Qwen 3.5 27B or Phi-4) — zero data leaves your machine.

---

## 13. Glossary

**MoE (Mixture of Experts)** — Architecture where only a subset of parameters activates per inference. Allows large model capacity with lower compute cost. Used in Llama 4, DeepSeek, Qwen 3.5, and Mixtral.

**Quantization** — Compressing model weights from 32-bit to 4-bit or 8-bit precision to reduce memory. Q4 fits larger models in less RAM with minor quality loss. Q8 is better quality, needs more RAM.

**GGUF** — File format for quantized models, used by Ollama and LLaMA.cpp. The standard for running models locally on consumer hardware.

**Ollama** — Free, open-source tool for downloading and running local AI models. One command per model.

**SWE-bench Verified** — A standard coding benchmark measuring a model's ability to resolve real GitHub software engineering issues. Industry-standard measure of coding capability.

**GPQA Diamond** — Graduate-level STEM reasoning benchmark. Questions require PhD-level expertise. The standard for measuring deep scientific reasoning.

**Artificial Analysis Intelligence Index** — Composite benchmark covering reasoning, knowledge, math, and coding across models. The most reliable single-number cross-provider comparison.

**MTok** — Million tokens. The unit of measurement for API pricing.

**RAG (Retrieval-Augmented Generation)** — Technique where a model retrieves relevant documents before generating an answer. Perplexity and NotebookLM use RAG to ground responses in real sources.

**Fine-tuning** — Retraining a model on your own data to improve performance for your specific use case. Only practical with open-weight models (Llama, Mistral, Qwen, etc.).

**VRAM** — Video RAM, the memory on a GPU. The primary constraint for running local models. Apple Silicon unified memory counts as VRAM.

**Data residency** — Where your data is physically stored and processed. EU data residency (Mistral) means your data stays within EU jurisdiction — important for GDPR compliance.

**Open-weight** — A model whose weights are publicly available for download, modification, and self-hosting. Not to be confused with "open-source," which implies training code and data are also available. Llama, Mistral, Qwen, Gemma, Phi, and DeepSeek are open-weight.

---

_Last updated: June 2026_ _Benchmarks, pricing, and features verified from official documentation and independent sources._ _Model landscape changes rapidly. Always verify current pricing at each provider before production deployment._ _Primary benchmarks: SWE-bench (coding), GPQA Diamond (reasoning), Artificial Analysis Intelligence Index (overall)._