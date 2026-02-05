# 🎓 AI Infrastructure Domain Learning Workflow

[![n8n](https://img.shields.io/badge/n8n-Workflow-orange?logo=n8n)](https://n8n.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Exa.ai](https://img.shields.io/badge/Search-Exa.ai-blue)](https://exa.ai)
[![OpenAI](https://img.shields.io/badge/AI-OpenAI-green?logo=openai)](https://openai.com)

> **Automated daily learning system for marketers building AI Infrastructure expertise**

![Workflow Preview](./assets/workflow-preview.png)

## 🎯 What This Does

This n8n workflow automatically curates, scores, and delivers AI Infrastructure learning content daily. Perfect for **B2B marketers transitioning into AI/ML GTM roles**.

**Key Features:**
- 📊 **Algorithmic Scoring** — Data-driven quality and difficulty scores
- 🎓 **Learning Paths** — Content categorized FOUNDATIONAL → ADVANCED
- 💼 **Marketing Focus** — Summaries highlight customer pain points & market trends
- 📱 **Daily Digest** — Telegram notifications with top picks

## 📦 What's Included

```
├── workflows/
│   ├── AI_Infrastructure_Learning_v7.json    # Main workflow
│   └── AI_Infrastructure_Learning_v6.json    # Previous version (simpler)
├── templates/
│   ├── google-sheets-template.xlsx           # Ready-to-use spreadsheet
│   └── keywords-examples.csv                 # Sample keywords
├── docs/
│   ├── SETUP.md                              # Detailed setup guide
│   ├── SCORING.md                            # Scoring methodology
│   └── CUSTOMIZATION.md                      # How to adapt for your domain
├── assets/
│   └── workflow-preview.png                  # Screenshots
└── README.md                                 # This file
```

## ⚡ Quick Start

### Prerequisites

- [n8n](https://n8n.io) (self-hosted or cloud)
- [Google Sheets](https://sheets.google.com) account
- [Exa.ai](https://exa.ai) API key
- [OpenAI](https://platform.openai.com) API key
- [Telegram](https://telegram.org) bot (optional)

### Installation

1. **Clone this repo**
   ```bash
   git clone https://github.com/yourusername/ai-infrastructure-learning-workflow.git
   ```

2. **Import workflow to n8n**
   - Open n8n → Workflows → Import from File
   - Select `workflows/AI_Infrastructure_Learning_v7.json`

3. **Set up Google Sheet**
   - Copy the template from `templates/google-sheets-template.xlsx`
   - Or create manually with these tabs:
     - `Keywords` (input)
     - `Sheet4` (output)

4. **Configure credentials**
   - Google Sheets OAuth2
   - Exa.ai API key (in HTTP Request header)
   - OpenAI API key
   - Telegram Bot Token + Chat ID

5. **Add your keywords**
   - Edit the Keywords tab with your target topics
   - Set Priority = "High" and Status = "Active"

6. **Test & Activate**
   ```
   Execute Workflow → Check outputs → Toggle Active
   ```

## 📊 Scoring System

### Quality Score (1-10)

| Component | Points | Description |
|-----------|--------|-------------|
| Source Authority | 0-3 | Domain reputation (arxiv, nvidia = 3pts) |
| Content Depth | 0-3 | Word count + structure |
| Marketing Relevance | 0-2 | GTM-useful terms (ROI, enterprise, etc.) |
| Recency | 0-2 | Publication date freshness |

### Difficulty Score (1-10)

| Score | Level | Audience |
|-------|-------|----------|
| 1-3 | 🟢 Easy | Anyone |
| 4-5 | 🟡 Moderate | Some AI exposure |
| 6-7 | 🟡 Challenging | Solid AI foundations |
| 8-9 | 🔴 Hard | Engineering background |
| 10 | 🔬 Expert | PhD-level |

[📖 Full scoring methodology →](./docs/SCORING.md)

## 🔧 Configuration

### Keywords Sheet Structure

| Keyword | Industry | Sub_Industry | Priority | Status |
|---------|----------|--------------|----------|--------|
| model serving | AI Infrastructure | Model Serving | High | Active |
| vLLM | AI Infrastructure | Model Serving | High | Active |
| inference cost | AI Infrastructure | Cost Optimization | High | Active |

### Output Sheet Structure

| Date | Title | Category | Difficulty | Quality_Score | Learning Summary | Prerequisites | URL |
|------|-------|----------|------------|---------------|------------------|---------------|-----|

## 💰 Cost Estimate

| Service | Monthly Usage | Cost |
|---------|---------------|------|
| Exa.ai | ~300 searches | ~$3-5 |
| OpenAI | ~300 summaries | ~$1-2 |
| Google Sheets | Unlimited | Free |
| Telegram | Unlimited | Free |
| **Total** | | **~$5-7/month** |

## 🎨 Customization

### For Different Industries

```javascript
// In "Aggregate & Score" node, update these:

const AUTHORITY_TIERS = {
  tier1: ['your-industry-leaders.com'],
  // ...
};

const TECHNICAL_TERMS = {
  advanced: ['your-advanced-terms'],
  intermediate: ['your-intermediate-terms'],
  foundational: ['your-basic-terms']
};
```

### Example Keyword Sets

**FinTech:**
```
payment processing, fraud detection, KYC automation, open banking
```

**Climate Tech:**
```
carbon capture, renewable energy storage, EV charging, green hydrogen
```

**DevTools:**
```
developer experience, CI/CD, observability, platform engineering
```

[📖 Full customization guide →](./docs/CUSTOMIZATION.md)

## 📱 Output Examples

### Telegram Digest

```
🎓 AI Infrastructure Learning Digest
📅 Wednesday, Feb 5

📊 Items: 10 | Avg Quality: 7.8/10 | Avg Difficulty: 6.2/10

⭐ TOP PICK
🔴 vLLM Performance Optimization for Production
   Difficulty: 8/10 | Quality: 9/10
   
   This article explains how vLLM achieves 24x throughput 
   improvement through PagedAttention...

📰 MORE TO READ
🟢 1. What is Model Serving? A Beginner's Guide
🟡 2. Inference Cost Optimization Strategies
...
```

### Google Sheet

![Google Sheet Output](./assets/sheet-output.png)

## 🤝 Contributing

Contributions welcome! Please read [CONTRIBUTING.md](./CONTRIBUTING.md) first.

**Ideas for contribution:**
- [ ] Add more source authority tiers
- [ ] Improve technical terms dictionary
- [ ] Add Slack integration
- [ ] Add Notion database output
- [ ] Create industry-specific keyword templates

## 📜 Changelog

### v7 (Current)
- ✅ Algorithmic scoring (Quality + Difficulty)
- ✅ GPT only writes summaries (doesn't override scores)
- ✅ Category auto-assignment
- ✅ Scoring methodology in Telegram digest

### v6
- Basic GPT-based scoring
- Manual category assignment

### v5
- Initial release
- RSS-style aggregation

## 🙏 Credits

- **Workflow Design:** [Your Name](https://linkedin.com/in/yourprofile)
- **Inspired by:** n8n RSS Aggregator template
- **Powered by:** [n8n](https://n8n.io), [Exa.ai](https://exa.ai), [OpenAI](https://openai.com)

## 📄 License

MIT License — see [LICENSE](./LICENSE) for details.

---

<p align="center">
  <b>Built for systematic learning</b> 🚀<br>
  <sub>Star ⭐ this repo if it helps your learning journey!</sub>
</p>
