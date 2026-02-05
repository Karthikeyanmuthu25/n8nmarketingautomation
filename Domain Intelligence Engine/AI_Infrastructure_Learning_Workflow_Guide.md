# 🎓 AI Infrastructure Domain Learning Workflow

> **Build deep AI Infrastructure expertise automatically** — A daily learning system for marketers transitioning into AI/ML GTM roles.

---

## 🧠 Overview

This workflow helps **B2B marketers build domain expertise in AI Infrastructure** by automatically curating, scoring, and delivering relevant learning content daily.

Instead of manually searching for articles across dozens of AI blogs, research papers, and tech publications, you define your target keywords once, and n8n delivers a **scored, categorized learning digest** every morning. Each article is algorithmically evaluated for difficulty level (can you understand it?) and quality (is it worth your time?), then organized into a structured learning path.

**Who is this for?**
- Marketers transitioning into AI/ML GTM roles
- GTM Engineers building domain credibility
- Anyone systematically learning AI Infrastructure concepts

**What makes this different?**
- **Algorithmic scoring** — Not just GPT opinions, but data-driven quality and difficulty scores
- **Learning-focused curation** — Content categorized by FOUNDATIONAL → INTERMEDIATE → ADVANCED → RESEARCH
- **Marketing-relevant insights** — Summaries highlight customer pain points, market trends, and competitive angles
- **Progressive skill building** — Prerequisites tell you what to learn first

---

## 📊 The Scoring System Explained

### Quality Score (1-10)

The Quality Score predicts **how valuable this article is for your learning time investment**.

| Component | Points | What It Measures |
|-----------|--------|------------------|
| **Source Authority** | 0-3 | Is this from a credible source? |
| **Content Depth** | 0-3 | Is there enough substance to learn from? |
| **Marketing Relevance** | 0-2 | Does it contain GTM-useful insights? |
| **Recency** | 0-2 | Is the information current? |

#### Source Authority Tiers

```
Tier 1 (3 pts): arxiv.org, openai.com, anthropic.com, deepmind.com, nvidia.com, google.ai
               → Primary research sources, official vendor docs

Tier 2 (2.5 pts): huggingface.co, pytorch.org, github.com, aws.amazon.com, cloud.google.com
                 → Major platforms, official engineering blogs

Tier 3 (2 pts): towardsdatascience.com, medium.com, techcrunch.com, venturebeat.com
               → Quality publications, curated platforms

Tier 4 (1.5 pts): reddit.com, stackoverflow.com, news.ycombinator.com
                 → Community sources (valuable but verify)

Bonus: .edu domains (3 pts), .gov domains (3 pts), .org domains (2 pts)
```

#### Content Depth Calculation

```
Word Count Scoring:
  > 2000 words = 3 pts (comprehensive deep-dive)
  > 1000 words = 2.5 pts (substantial article)
  > 500 words  = 2 pts (standard article)
  > 300 words  = 1.5 pts (brief overview)
  < 300 words  = 1 pt (snippet/summary)

Structure Bonuses:
  +0.3 pts for headings (organized content)
  +0.2 pts for bullet/numbered lists (scannable)
```

#### Marketing Relevance Terms

Articles containing these terms score higher because they're more actionable for GTM roles:

```
Business Impact: ROI, cost, pricing, budget, revenue, savings
Enterprise Focus: enterprise, production, scale, deployment, infrastructure
Customer Lens: customer, use case, case study, pain point, challenge, problem, solution
Market Context: market, adoption, trend, growth, forecast, comparison, versus, benchmark
```

```
5+ terms = 2 pts (highly relevant for GTM)
3-4 terms = 1.5 pts (good marketing angle)
1-2 terms = 1 pt (some relevance)
0 terms = 0.5 pts (purely technical)
```

#### Recency Scoring

```
≤ 7 days old  = 2 pts (breaking/current)
≤ 14 days old = 1.75 pts (recent)
≤ 30 days old = 1.5 pts (relevant)
≤ 60 days old = 1.25 pts (still useful)
> 60 days old = 1 pt (may be dated)
```

---

### Difficulty Score (1-10)

The Difficulty Score predicts **how much prior knowledge you need** to understand this article.

| Score | Level | Who Can Understand |
|-------|-------|-------------------|
| 1-3 | Easy | Anyone with basic tech literacy |
| 4-5 | Moderate | Marketers with some AI exposure |
| 6-7 | Challenging | Need solid AI/ML foundations |
| 8-9 | Hard | Requires engineering background |
| 10 | Expert | PhD-level / cutting-edge research |

#### How Difficulty Is Calculated

**Base Score: 3** (assumes moderate baseline)

**Technical Terms Detection:**

```javascript
ADVANCED TERMS (+2 to +4 points):
  transformer, attention mechanism, backpropagation, gradient descent,
  CUDA, kernel, tensor, eigenvalue, jacobian, hessian, KV cache,
  PagedAttention, FlashAttention, speculative decoding, continuous batching

INTERMEDIATE TERMS (+1 to +2 points):
  inference, latency, throughput, batch size, quantization, pruning,
  distillation, fine-tuning, embedding, vector, GPU, TPU, API,
  endpoint, containerization, kubernetes

FOUNDATIONAL TERMS (baseline, may reduce score):
  model, training, prediction, accuracy, dataset, parameter,
  neural network, machine learning, artificial intelligence, deep learning
```

**Additional Difficulty Signals:**

```
+2 pts: Code presence (```, def, class, import, function)
+2 pts: Mathematical notation (∑, ∏, ∫, √, LaTeX expressions, O(n) notation)
+1 pt:  Complex sentences (average > 25 words per sentence)
-2 pts: Mostly foundational terms with few advanced concepts
```

---

### Category Assignment

Based on difficulty score and content analysis:

```
RESEARCH:     Contains 'arxiv', 'paper', 'study shows', 'we propose', 'abstract'
ADVANCED:     Difficulty ≥ 8
INTERMEDIATE: Difficulty ≥ 5
FOUNDATIONAL: Difficulty < 5
```

**Visual Guide:**
- 🟢 **FOUNDATIONAL** — Start here if you're new to the topic
- 🟡 **INTERMEDIATE** — Good for building working knowledge
- 🔴 **ADVANCED** — Deep dives for technical credibility
- 🔬 **RESEARCH** — Cutting-edge papers and studies

---

## ⚙️ How The Workflow Works

### Architecture Diagram

```
┌─────────────────┐
│  Daily 9am      │
│  Trigger        │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Read Keywords  │ ← From Google Sheets (Keywords tab)
│  Sheet          │   Filters: Active + High Priority
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Generate       │ ← Creates 7 query types per keyword
│  Learning       │   (foundational, market, pain points,
│  Queries        │    competitive, ROI, technical, news)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Exa Neural     │ ← AI-powered semantic search
│  Search         │   3 results per query, last 30 days
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Wait 2s        │ ← Rate limiting protection
└────────┬────────┘
         │
         ▼
┌─────────────────────────┐
│  Aggregate & Score      │ ← ALGORITHMIC SCORING
│  (Algorithm)            │   • Quality Score (1-10)
│                         │   • Difficulty Score (1-10)
│                         │   • Category Assignment
└────────┬────────────────┘
         │
         ▼
┌─────────────────┐
│  GPT Write      │ ← Only writes summaries
│  Summaries      │   Scores already calculated
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Format Output  │
└────────┬────────┘
         │
    ┌────┴────┐
    │         │
    ▼         ▼
┌───────┐ ┌──────────┐
│ Save  │ │ Telegram │
│ to    │ │ Digest   │
│ Sheet │ │          │
└───────┘ └──────────┘
```

### Node-by-Node Breakdown

#### 1. Schedule Trigger
- **What:** Runs workflow automatically
- **Default:** Daily at 9:00 AM
- **Customize:** Change cron expression for different times/frequencies

#### 2. Read Keywords
- **Source:** Google Sheets → Keywords tab
- **Filters applied:** 
  - `Status (Active/Inactive)` = "Active"
  - `Priority` = "High"
- **Input columns:** Keyword, Industry, Sub_Industry, Priority, Status

#### 3. Generate Learning Queries
Creates 7 different query types for comprehensive coverage:

| Query Type | Purpose | Example Query |
|------------|---------|---------------|
| Foundational | Understand basics | "what is model serving explained simply for beginners" |
| Market | Industry dynamics | "vLLM market trends enterprise adoption 2024 2025" |
| Pain Points | Customer problems | "inference latency challenges problems enterprises face" |
| Competitive | Landscape mapping | "GPU utilization comparison vendors tools platforms" |
| ROI | Proof points | "quantization use cases ROI benefits case studies" |
| Technical | Build credibility | "RAG architecture best practices implementation" |
| Research | Stay current | "vector database latest news announcements" |

#### 4. Exa Neural Search
- **API:** Exa.ai (semantic/neural search)
- **Results per query:** 3
- **Date range:** Last 30 days
- **Content returned:** Full text (2500 chars), highlights, AI summary

#### 5. Aggregate & Score (Algorithm)
**This is where the magic happens.** Pure JavaScript scoring:
- Deduplicates URLs across all queries
- Calculates Quality Score (source + depth + marketing + recency)
- Calculates Difficulty Score (technical terms + code + math + complexity)
- Assigns Category (FOUNDATIONAL/INTERMEDIATE/ADVANCED/RESEARCH)
- Sorts by Quality Score descending
- Keeps top 10 items

#### 6. GPT Write Summaries
- **Model:** GPT-4o-mini
- **Purpose:** Write human-readable Learning Summaries
- **Important:** Does NOT change the algorithmic scores
- **Output:** 2-3 sentence summary + prerequisites per article

#### 7. Format Output
- Prepares data for Google Sheets (exact column mapping)
- Builds Telegram message with formatting
- Includes scoring methodology in digest

#### 8. Save to Sheet4
- **Operation:** Append (adds new rows)
- **Columns:** Date, Title, Category, Difficulty, Quality_Score, Learning Summary, Prerequisites, URL

#### 9. Send Telegram
- Sends formatted digest to your Telegram
- Includes: stats, top pick, more to read, scoring methodology

---

## 📋 Google Sheets Structure

### Keywords Tab (Input)

| Column | Type | Example | Required |
|--------|------|---------|----------|
| Keyword | Text | "model serving" | ✅ |
| Industry | Text | "AI Infrastructure" | ✅ |
| Sub_Industry | Text | "Model Serving" | Optional |
| Priority | Dropdown | High / Medium / Low | ✅ |
| Status (Active/Inactive) | Dropdown | Active / Inactive | ✅ |

**Example Keywords for AI Infrastructure:**

| Keyword | Industry | Sub_Industry | Priority | Status |
|---------|----------|--------------|----------|--------|
| model serving | AI Infrastructure | Model Serving | High | Active |
| vLLM | AI Infrastructure | Model Serving | High | Active |
| inference cost | AI Infrastructure | Cost Optimization | High | Active |
| inference latency | AI Infrastructure | Model Serving | High | Active |
| quantization | AI Infrastructure | Inference Optimization | High | Active |
| model compression | AI Infrastructure | Inference Optimization | High | Active |
| vector database | AI Infrastructure | Vector Databases | High | Active |
| RAG architecture | AI Infrastructure | RAG Infrastructure | High | Active |
| GPU utilization | AI Infrastructure | GPU Management | High | Active |
| GPU memory | AI Infrastructure | GPU Management | High | Active |

### Sheet4 Tab (Output)

| Column | Type | Description |
|--------|------|-------------|
| Date (datetime) | ISO DateTime | When the article was discovered |
| Title | Text | Article title (max 100 chars) |
| Category | Enum | FOUNDATIONAL / INTERMEDIATE / ADVANCED / RESEARCH |
| Difficulty (1-10) | Number | Algorithmic difficulty score |
| Quality_Score (1-10) | Number | Algorithmic quality score |
| Learning Summary | Text | GPT-generated 2-3 sentence summary |
| Prerequisites | Text | What to learn first |
| URL | URL | Link to the article |

---

## 📱 Telegram Digest Format

```
🎓 AI Infrastructure Learning Digest
📅 Wednesday, Feb 5

━━━━━━━━━━━━━━━━━━━━━━━━━
📊 SCORING: ALGORITHMIC
━━━━━━━━━━━━━━━━━━━━━━━━━

📚 Items: 10
📈 Avg Quality: 7.8/10
🎯 Avg Difficulty: 6.2/10
⏱️ Est. Read Time: ~50 mins

By Level:
🟢 Foundational: 2
🟡 Intermediate: 5
🔴 Advanced: 2
🔬 Research: 1

━━━━━━━━━━━━━━━━━━━━━━━━━
⭐ TOP PICK
━━━━━━━━━━━━━━━━━━━━━━━━━

🔴 vLLM Performance Optimization for Production

📊 ADVANCED
🎯 Difficulty: 8/10
⭐ Quality: 9/10

📝 Summary:
This article explains how vLLM achieves 24x throughput improvement 
through PagedAttention. It matters because inference cost is the 
#1 concern for enterprises. Marketing insight: customers care 
most about cost-per-token reduction.

📚 Prerequisites: Understanding of transformer architecture, 
basic knowledge of GPU memory management

🔗 Read Article

━━━━━━━━━━━━━━━━━━━━━━━━━
📰 MORE TO READ
━━━━━━━━━━━━━━━━━━━━━━━━━

🟢 2. What is Model Serving? A Beginner's Guide
   FOUNDATIONAL | Diff: 3/10 | Quality: 8/10
   💡 Model serving is the process of deploying ML models...
   🔗 Read

🟡 3. Inference Cost Optimization Strategies
   INTERMEDIATE | Diff: 6/10 | Quality: 8/10
   💡 Enterprises spend 70% of ML budget on inference...
   🔗 Read

... (more articles)

━━━━━━━━━━━━━━━━━━━━━━━━━
📐 SCORING METHODOLOGY
━━━━━━━━━━━━━━━━━━━━━━━━━

Quality Score (1-10):
• Source Authority: 0-3 pts
• Content Depth: 0-3 pts
• Marketing Relevance: 0-2 pts
• Recency: 0-2 pts

Difficulty Score (1-10):
• Technical terms density
• Code/math presence
• Sentence complexity

_Algorithmic scoring for consistency_ 🤖
```

---

## 🧩 Integrations Required

| Service | Purpose | Credentials Needed |
|---------|---------|-------------------|
| **Google Sheets** | Store keywords (input) and results (output) | OAuth2 |
| **Exa.ai** | Neural/semantic web search | API Key |
| **OpenAI** | Generate learning summaries | API Key |
| **Telegram** | Deliver daily digest | Bot Token + Chat ID |

### API Costs Estimate

| Service | Usage | Estimated Cost |
|---------|-------|----------------|
| Exa.ai | ~10 searches/day × 30 days | ~$3-5/month |
| OpenAI (GPT-4o-mini) | ~10 summaries/day × 30 days | ~$1-2/month |
| Google Sheets | Free tier | $0 |
| Telegram | Free | $0 |

**Total: ~$5-7/month** for daily automated learning curation.

---

## 🚀 Setup Instructions

### Step 1: Create Google Sheet

1. Create a new Google Sheet
2. Rename first tab to "Keywords"
3. Add headers: `Keyword | Industry | Sub_Industry | Priority | Status (Active/Inactive)`
4. Add your target keywords (see examples above)
5. Create second tab named "Sheet4"
6. Add headers: `Date (datetime) | Title | Category (FOUNDATIONAL/INTERMEDIATE/ADVANCED/RESEARCH) | Difficulty (1-10) | Quality_Score (1-10) | Learning Summary | Prerequisites | URL`

### Step 2: Get API Keys

**Exa.ai:**
1. Go to [exa.ai](https://exa.ai)
2. Sign up and get API key
3. Add to HTTP Request node header

**OpenAI:**
1. Go to [platform.openai.com](https://platform.openai.com)
2. Create API key
3. Add as n8n OpenAI credential

**Telegram Bot:**
1. Message @BotFather on Telegram
2. Create new bot, get token
3. Get your chat ID (message @userinfobot)
4. Add as n8n Telegram credential

### Step 3: Import Workflow

1. Download the workflow JSON
2. In n8n: Workflows → Import from File
3. Select the JSON file
4. Update credentials for each node
5. Update Google Sheet document ID
6. Update Telegram chat ID

### Step 4: Test & Activate

1. Click "Execute Workflow" to test manually
2. Check Google Sheet for new rows
3. Check Telegram for digest message
4. If working, toggle workflow to "Active"

---

## 💡 Customization Ideas

### Change Your Domain

Replace AI Infrastructure keywords with your industry:

**FinTech Example:**
```
Keywords: payment processing, fraud detection, KYC automation, 
          open banking, embedded finance, BNPL, neobank
```

**Climate Tech Example:**
```
Keywords: carbon capture, renewable energy storage, grid optimization,
          EV charging infrastructure, green hydrogen, carbon credits
```

**DevTools Example:**
```
Keywords: developer experience, CI/CD pipeline, observability,
          infrastructure as code, platform engineering, API gateway
```

### Adjust Scoring Weights

In the "Aggregate & Score" node, modify these sections:

```javascript
// Add your industry's authority sources
const AUTHORITY_TIERS = {
  tier1: ['your-industry-leaders.com'],
  tier2: ['secondary-sources.com'],
  // ...
};

// Add your industry's technical terms
const TECHNICAL_TERMS = {
  advanced: ['your-advanced-terms'],
  intermediate: ['your-intermediate-terms'],
  foundational: ['your-basic-terms']
};

// Add your marketing-relevant terms
const MARKETING_TERMS = ['your', 'gtm', 'relevant', 'terms'];
```

### Add More Output Channels

**Slack Daily Digest:**
```
Format Output → Slack Node → #learning-channel
```

**Email Summary:**
```
Format Output → Gmail/SMTP Node → Your inbox
```

**Notion Database:**
```
Format Output → Notion Node → Learning Database
```

### Add AI Enhancements

**Priority Scoring:**
Add a field rating urgency (1-10) based on:
- Mentions of your company/competitors
- Breaking news indicators
- Trending topics

**Content Clustering:**
Group similar articles together using embeddings

**Reading Time Estimate:**
Calculate based on word count (~200 words/minute)

---

## ⚠️ Troubleshooting

### "No active keywords found"
- Check Keywords sheet has rows with Status = "Active" AND Priority = "High"
- Verify column headers match exactly (case-sensitive)

### Exa search returns no results
- Check API key is valid
- Try broader keywords
- Increase `num_results` or extend date range

### GPT parsing fails
- Workflow has fallback logic
- Check OpenAI API key and quota
- Review GPT response in execution log

### Google Sheets not updating
- Verify OAuth credentials are connected
- Check Sheet ID matches your document
- Confirm "Sheet4" tab exists with correct headers

### Telegram not receiving messages
- Verify bot token is correct
- Confirm chat ID (use @userinfobot)
- Check bot has permission to message you (start a conversation first)

---

## 📈 Measuring Your Learning Progress

Track these metrics over time in your Google Sheet:

| Metric | How to Track | Goal |
|--------|--------------|------|
| Articles read | Count rows marked "read" | 5-10/week |
| Avg difficulty handled | Average of articles you understood | Increase over time |
| Category distribution | Pie chart of FOUND/INT/ADV/RES | More ADVANCED over time |
| Topics covered | Unique keywords learned | Expand breadth |
| Quality threshold | Min quality you engage with | Raise over time |

**Suggested Learning Path:**

```
Week 1-4:   Focus on FOUNDATIONAL (🟢) articles
Week 5-8:   Mix FOUNDATIONAL + INTERMEDIATE (🟡)
Week 9-12:  Primarily INTERMEDIATE + some ADVANCED (🔴)
Week 13+:   Comfortable with ADVANCED, explore RESEARCH (🔬)
```

---

## 🔧 Workflow Maintenance

### Weekly
- Review Keywords sheet — add emerging topics, pause saturated ones
- Check execution logs for errors
- Skim Sheet4 for quality of curation

### Monthly
- Analyze which keywords produce best content
- Update authority tiers with new sources you've discovered
- Adjust marketing relevance terms based on your GTM focus

### Quarterly
- Review scoring methodology — is it still calibrated correctly?
- Archive old Sheet4 data to keep it manageable
- Consider adding new output channels (Slack, email, Notion)

---

## 💬 Credits & Support

**Workflow Author:** [Your Name]
**Based on:** n8n RSS Aggregator template
**Purpose:** Marketer → AI Infrastructure expertise building

**Need Help?**
- n8n Community: [community.n8n.io](https://community.n8n.io)
- Exa.ai Docs: [docs.exa.ai](https://docs.exa.ai)
- OpenAI API Docs: [platform.openai.com/docs](https://platform.openai.com/docs)

---

## 📄 License

MIT License — Use freely, modify as needed, attribution appreciated.

---

*Built with n8n, Exa.ai, OpenAI, and a passion for systematic learning.* 🚀
