# 📊 Scoring Methodology

> Deep dive into how Quality Score and Difficulty Score are calculated

## Overview

This workflow uses **algorithmic scoring** instead of relying solely on LLM judgment. This provides:

- **Consistency** — Same content = same score every time
- **Transparency** — You know exactly why something scored high/low
- **Customizability** — Adjust weights for your specific needs

---

## Quality Score (1-10)

The Quality Score predicts **how valuable this article is for your learning time investment**.

### Formula

```
Quality Score = Source Authority + Content Depth + Marketing Relevance + Recency
             = (0-3)          + (0-3)        + (0-2)              + (0-2)
             = 0 to 10
```

---

### 1. Source Authority (0-3 points)

Measures the credibility and reputation of the source domain.

#### Tier System

| Tier | Points | Domains | Rationale |
|------|--------|---------|-----------|
| **Tier 1** | 3.0 | arxiv.org, openai.com, anthropic.com, deepmind.com, nvidia.com, google.ai, research.google | Primary research, official vendor documentation |
| **Tier 2** | 2.5 | huggingface.co, pytorch.org, tensorflow.org, github.com, aws.amazon.com, cloud.google.com, azure.microsoft.com | Major platforms, official engineering blogs |
| **Tier 3** | 2.0 | towardsdatascience.com, medium.com, dev.to, techcrunch.com, venturebeat.com, theregister.com | Quality publications, curated content platforms |
| **Tier 4** | 1.5 | reddit.com, stackoverflow.com, news.ycombinator.com | Community sources (valuable but verify independently) |
| **Default** | 1.0 | All other domains | Unknown authority |

#### Special Cases

| Domain Type | Points | Examples |
|-------------|--------|----------|
| `.edu` | 3.0 | stanford.edu, mit.edu |
| `.gov` | 3.0 | nist.gov, energy.gov |
| `.org` | 2.0 | mozilla.org, apache.org |

#### Code Implementation

```javascript
function getAuthorityScore(url) {
  const domain = new URL(url).hostname.replace('www.', '');
  
  if (AUTHORITY_TIERS.tier1.some(d => domain.includes(d))) return 3;
  if (AUTHORITY_TIERS.tier2.some(d => domain.includes(d))) return 2.5;
  if (AUTHORITY_TIERS.tier3.some(d => domain.includes(d))) return 2;
  if (AUTHORITY_TIERS.tier4.some(d => domain.includes(d))) return 1.5;
  if (domain.includes('.edu') || domain.includes('.gov')) return 3;
  if (domain.includes('.org')) return 2;
  
  return 1; // Default
}
```

---

### 2. Content Depth (0-3 points)

Measures whether there's enough substance to learn from.

#### Word Count Scoring

| Word Count | Points | Content Type |
|------------|--------|--------------|
| > 2000 | 3.0 | Comprehensive deep-dive |
| > 1000 | 2.5 | Substantial article |
| > 500 | 2.0 | Standard article |
| > 300 | 1.5 | Brief overview |
| < 300 | 1.0 | Snippet/summary |

#### Structure Bonuses

| Element | Bonus | Detection Method |
|---------|-------|------------------|
| Headings | +0.3 | Regex: `/#|##|###|<h[1-6]>/i` |
| Lists | +0.2 | Regex: `/^[-*•]|\n[-*•]/m` or `/^\d+\.|\n\d+\./m` |

#### Code Implementation

```javascript
function getDepthScore(text) {
  const wordCount = text.split(/\s+/).length;
  let score = 0;
  
  if (wordCount > 2000) score = 3;
  else if (wordCount > 1000) score = 2.5;
  else if (wordCount > 500) score = 2;
  else if (wordCount > 300) score = 1.5;
  else score = 1;
  
  // Structure bonuses
  if (/#|##|###|<h[1-6]>/i.test(text)) score += 0.3;
  if (/^[-*•]|\n[-*•]/m.test(text)) score += 0.2;
  
  return Math.min(score, 3); // Cap at 3
}
```

---

### 3. Marketing Relevance (0-2 points)

Measures how useful the content is for GTM/marketing purposes.

#### Marketing Terms Dictionary

```javascript
const MARKETING_TERMS = [
  // Business Impact
  'ROI', 'cost', 'pricing', 'budget', 'revenue', 'savings',
  
  // Enterprise Focus
  'enterprise', 'production', 'scale', 'deployment', 'infrastructure',
  
  // Customer Lens
  'customer', 'use case', 'case study', 'pain point', 
  'challenge', 'problem', 'solution',
  
  // Market Context
  'market', 'adoption', 'trend', 'growth', 'forecast', 
  'comparison', 'versus', 'vs', 'benchmark'
];
```

#### Scoring Matrix

| Terms Found | Points | Interpretation |
|-------------|--------|----------------|
| 5+ | 2.0 | Highly relevant for GTM |
| 3-4 | 1.5 | Good marketing angle |
| 1-2 | 1.0 | Some relevance |
| 0 | 0.5 | Purely technical |

---

### 4. Recency (0-2 points)

Measures how current the information is.

| Days Since Published | Points | Freshness |
|---------------------|--------|-----------|
| ≤ 7 | 2.0 | Breaking/current |
| ≤ 14 | 1.75 | Recent |
| ≤ 30 | 1.5 | Relevant |
| ≤ 60 | 1.25 | Still useful |
| > 60 | 1.0 | May be dated |

#### Code Implementation

```javascript
function getRecencyScore(publishedDate) {
  if (!publishedDate) return 1;
  
  const daysSince = Math.floor(
    (Date.now() - new Date(publishedDate)) / (1000 * 60 * 60 * 24)
  );
  
  if (daysSince <= 7) return 2;
  if (daysSince <= 14) return 1.75;
  if (daysSince <= 30) return 1.5;
  if (daysSince <= 60) return 1.25;
  return 1;
}
```

---

## Difficulty Score (1-10)

The Difficulty Score predicts **how much prior knowledge you need** to understand the content.

### Formula

```
Difficulty Score = Base + Technical Terms + Code + Math + Complexity - Foundational Adjustment
                 = 3    + (0-4)          + (0-2)+ (0-2)+ (0-1)     - (0-2)
                 = 1 to 10 (clamped)
```

---

### Technical Terms Detection

Three tiers of technical vocabulary are tracked:

#### Advanced Terms (+2 to +4 points)

Concepts requiring deep ML/systems knowledge:

```javascript
const ADVANCED_TERMS = [
  // ML Architecture
  'transformer', 'attention mechanism', 'backpropagation', 
  'gradient descent', 'eigenvalue', 'jacobian', 'hessian',
  
  // Systems/CUDA
  'CUDA', 'kernel', 'tensor',
  
  // Inference Optimization
  'KV cache', 'PagedAttention', 'FlashAttention', 
  'speculative decoding', 'continuous batching'
];
```

| Count | Points Added |
|-------|--------------|
| ≥ 3 | +4 |
| ≥ 1 | +2 |
| 0 | +0 |

#### Intermediate Terms (+1 to +2 points)

Industry-standard concepts:

```javascript
const INTERMEDIATE_TERMS = [
  // ML Operations
  'inference', 'latency', 'throughput', 'batch size',
  
  // Optimization Techniques
  'quantization', 'pruning', 'distillation', 'fine-tuning',
  
  // Data/Vectors
  'embedding', 'vector',
  
  // Infrastructure
  'GPU', 'TPU', 'API', 'endpoint', 
  'containerization', 'kubernetes'
];
```

| Count | Points Added |
|-------|--------------|
| ≥ 5 | +2 |
| ≥ 2 | +1 |
| 0 | +0 |

#### Foundational Terms (baseline)

Basic concepts that indicate accessible content:

```javascript
const FOUNDATIONAL_TERMS = [
  'model', 'training', 'prediction', 'accuracy', 'dataset',
  'parameter', 'neural network', 'machine learning', 
  'artificial intelligence', 'deep learning'
];
```

**Adjustment Rule:** If `foundationalCount > advancedCount * 2` AND `advancedCount < 2`, subtract 2 points (content is accessible despite some jargon).

---

### Code Presence (+2 points)

Detected via patterns:

```javascript
const hasCode = text.includes('```') || 
                text.includes('def ') || 
                text.includes('class ') || 
                text.includes('import ') || 
                text.includes('function ');
```

---

### Mathematical Notation (+2 points)

Detected via:

```javascript
const hasMath = /[∑∏∫√∂∇]|\\frac|\\sum|O\(n\)|O\(1\)/i.test(text);
```

Catches:
- Unicode math symbols (∑, ∏, ∫, √, ∂, ∇)
- LaTeX notation (\frac, \sum)
- Big O notation (O(n), O(1))

---

### Sentence Complexity (+1 point)

```javascript
const avgSentenceLength = text.split(/[.!?]/)
  .reduce((sum, s) => sum + s.split(' ').length, 0) 
  / Math.max(text.split(/[.!?]/).length, 1);

if (avgSentenceLength > 25) score += 1;
```

Academic and technical writing tends to have longer sentences.

---

### Final Calculation

```javascript
function calculateDifficultyScore(text, title) {
  let score = 3; // Base
  
  const fullText = (title + ' ' + text).toLowerCase();
  
  // Count terms
  let advancedCount = ADVANCED_TERMS.filter(t => 
    fullText.includes(t.toLowerCase())
  ).length;
  
  let intermediateCount = INTERMEDIATE_TERMS.filter(t => 
    fullText.includes(t.toLowerCase())
  ).length;
  
  let foundationalCount = FOUNDATIONAL_TERMS.filter(t => 
    fullText.includes(t.toLowerCase())
  ).length;
  
  // Apply scoring
  if (advancedCount >= 3) score += 4;
  else if (advancedCount >= 1) score += 2;
  
  if (intermediateCount >= 5) score += 2;
  else if (intermediateCount >= 2) score += 1;
  
  if (hasCode(text)) score += 2;
  if (hasMath(text)) score += 2;
  if (avgSentenceLength(text) > 25) score += 1;
  
  // Foundational adjustment
  if (foundationalCount > advancedCount * 2 && advancedCount < 2) {
    score -= 2;
  }
  
  return Math.min(Math.max(score, 1), 10); // Clamp 1-10
}
```

---

## Category Assignment

Based on difficulty score and content signals:

```javascript
function determineCategory(difficultyScore, text) {
  const textLower = text.toLowerCase();
  
  // Research indicators
  const isResearch = textLower.includes('arxiv') || 
                     textLower.includes('paper') || 
                     textLower.includes('study shows') ||
                     textLower.includes('we propose') ||
                     textLower.includes('abstract');
  
  if (isResearch) return 'RESEARCH';
  if (difficultyScore >= 8) return 'ADVANCED';
  if (difficultyScore >= 5) return 'INTERMEDIATE';
  return 'FOUNDATIONAL';
}
```

| Category | Criteria | Icon |
|----------|----------|------|
| RESEARCH | Contains research paper indicators | 🔬 |
| ADVANCED | Difficulty ≥ 8 | 🔴 |
| INTERMEDIATE | Difficulty ≥ 5 | 🟡 |
| FOUNDATIONAL | Difficulty < 5 | 🟢 |

---

## Customizing for Your Domain

### Adding Authority Sources

```javascript
const AUTHORITY_TIERS = {
  tier1: [
    ...existingTier1,
    'your-industry-leader.com',
    'official-vendor.io'
  ],
  // ...
};
```

### Adding Technical Terms

```javascript
const TECHNICAL_TERMS = {
  advanced: [
    ...existingAdvanced,
    'your-advanced-term',
    'industry-specific-jargon'
  ],
  // ...
};
```

### Adjusting Marketing Terms

```javascript
const MARKETING_TERMS = [
  ...existingTerms,
  'your-gtm-term',
  'industry-buying-signal'
];
```

---

## Score Interpretation Guide

### Quality Score

| Score | Interpretation | Action |
|-------|----------------|--------|
| 9-10 | Exceptional | Must read |
| 7-8 | High quality | Read this week |
| 5-6 | Decent | Skim or save for later |
| 3-4 | Low quality | Skip unless very relevant |
| 1-2 | Poor | Ignore |

### Difficulty Score

| Score | Your Level | Action |
|-------|------------|--------|
| 1-3 | Beginner | Start here |
| 4-5 | Building foundations | Good next step |
| 6-7 | Solid understanding | Challenge yourself |
| 8-9 | Advanced practitioner | Deep dive |
| 10 | Expert | Research-level |

### Reading Strategy by Category

```
Week 1-4:   🟢 FOUNDATIONAL → Build vocabulary
Week 5-8:   🟡 INTERMEDIATE → Understand market
Week 9-12:  🔴 ADVANCED → Technical credibility
Week 13+:   🔬 RESEARCH → Cutting edge
```

---

## Validation & Testing

To validate scoring is working correctly:

1. **Manual spot check**: Read 5 articles, score them yourself, compare to algorithm
2. **Distribution check**: Scores should be roughly normal (most 4-7, few 1-2 or 9-10)
3. **Source consistency**: Same source should score similar authority each time
4. **Difficulty calibration**: Advanced papers should consistently score 7+

---

*Questions about scoring? Open an issue or PR to improve the methodology.*
