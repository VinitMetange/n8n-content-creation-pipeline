# 🎬 AI Content Creation Pipeline

> **Automated end-to-end content factory powered by n8n**
>
> Research any topic with Perplexity AI → Generate professional script with ChatGPT → Create AI video with HeyGen → Auto-publish to YouTube

[![n8n](https://img.shields.io/badge/Automation-n8n-FF6D5A?style=flat-square&logo=n8n)](https://n8n.io)
[![Perplexity](https://img.shields.io/badge/Research-Perplexity_AI-20B2AA?style=flat-square)](#)
[![ChatGPT](https://img.shields.io/badge/Script-ChatGPT_GPT--4o-412991?style=flat-square&logo=openai)](#)
[![HeyGen](https://img.shields.io/badge/Video-HeyGen_AI-FF4500?style=flat-square)](#)
[![YouTube](https://img.shields.io/badge/Publish-YouTube-FF0000?style=flat-square&logo=youtube)](#)
[![LinkedIn](https://img.shields.io/badge/Author-Vinit_Metange-blue?style=flat-square&logo=linkedin)](https://linkedin.com/in/vinit-metange)

---

## 🎯 Pipeline Overview

This n8n workflow automates the **entire content creation lifecycle** — from topic ideation to published video — reducing manual effort from hours to minutes.

```
┌────────────────────────────────────────────────────────────────────────┐
│  TRIGGER  │  RESEARCH   │  SCRIPTING  │  VIDEO GEN  │  PUBLISH  │
│          │             │             │             │           │
│ Schedule → Perplexity  →  ChatGPT    →   HeyGen    →  YouTube  │
│ or Topic  │ AI Search   │  GPT-4o    │  AI Avatar  │  Data API │
└──────────┴─────────────┴─────────────┴─────────────┴─────────┘
```

---

## 🔄 Pipeline Stages

### Stage 1: 🔍 Topic Research — Perplexity AI

**Node**: HTTP Request → Perplexity API

- Input: Topic keyword or title (manual trigger or scheduled)
- Perplexity `sonar-pro` model performs real-time web search
- Extracts: key facts, statistics, recent developments, expert quotes
- Output: Structured research brief (JSON) with citations

```json
{
  "topic": "Agentic AI in 2025",
  "key_points": ["...fact 1", "...fact 2"],
  "statistics": ["30% cost savings", "4x faster deployment"],
  "sources": ["techcrunch.com", "arxiv.org"]
}
```

---

### Stage 2: ✏️ Script Generation — ChatGPT (GPT-4o)

**Node**: OpenAI Node → GPT-4o

- Receives research brief from Stage 1
- System prompt: YouTube content creator persona with hook, body, CTA structure
- Generates:
  - **Hook** (first 3 seconds — attention grabbing)
  - **Introduction** (topic context)
  - **Main Content** (3-5 key points with examples)
  - **Call to Action** (subscribe, comment, share)
- Output: Full video script optimized for speaking pace (150 words/min)

```
Hook:         "Did you know AI agents can now do your entire job?"
Intro:        Context and why this matters
Main Points:  3-5 structured sections with transitions
CTA:          "Subscribe for weekly AI insights"
```

---

### Stage 3: 🎬 Video Generation — HeyGen AI

**Node**: HTTP Request → HeyGen API

- Sends script to HeyGen's video generation API
- Configured with:
  - **AI Avatar**: Professional presenter avatar
  - **Voice**: Natural TTS voice (English, Hindi options)
  - **Background**: Professional studio / tech background
  - **Captions**: Auto-generated subtitles
- Polls for video completion (webhook or polling loop)
- Downloads rendered MP4 video file
- Output: Rendered video with avatar presenting the script

```json
{
  "avatar_id": "professional_male_01",
  "voice_id": "en-US-neural",
  "script": "<generated_script>",
  "background": "tech_studio",
  "caption": true
}
```

---

### Stage 4: 🚀 YouTube Upload — YouTube Data API v3

**Node**: HTTP Request → YouTube Data API

- Authenticates via OAuth2
- Uploads video with auto-generated metadata:
  - **Title**: SEO-optimized from script topic
  - **Description**: Research sources + timestamps + social links
  - **Tags**: Extracted keywords from research brief
  - **Thumbnail**: HeyGen-generated thumbnail or Canva API
  - **Category**: Science & Technology (28)
  - **Visibility**: Scheduled or Public
- Sends Slack/email notification on successful publish

---

## 🛠️ Tech Stack

| Component | Tool | Purpose |
|-----------|------|---------|
| 🤖 Automation | n8n (self-hosted / cloud) | Workflow orchestration |
| 🔍 Research | Perplexity AI (sonar-pro) | Real-time web research |
| ✏️ Script | OpenAI GPT-4o | Script writing & optimization |
| 🎬 Video | HeyGen API | AI avatar video generation |
| 📺 Publish | YouTube Data API v3 | Video upload & metadata |
| 📧 Notifications | Slack / Gmail node | Status alerts |
| 💾 Storage | Google Drive / S3 | Video asset storage |

---

## 📁 Repository Structure

```
n8n-content-creation-pipeline/
├── workflows/
│   ├── content-pipeline-main.json      # Main n8n workflow export
│   ├── research-only.json              # Standalone research workflow
│   └── script-generator.json           # Standalone script workflow
├── prompts/
│   ├── system-prompt-scriptwriter.md   # ChatGPT system prompt
│   ├── research-extraction-prompt.md   # Perplexity parsing prompt
│   └── seo-title-description.md        # YouTube metadata prompt
├── configs/
│   ├── heygen-avatars.json             # Available avatar configs
│   └── youtube-categories.json         # YouTube category mapping
└── docs/
    ├── setup-guide.md                  # Step-by-step setup
    └── api-credentials.md              # API key configuration guide
```

---

## ⚡ Quick Start

### Prerequisites

- n8n instance (self-hosted or n8n.cloud)
- API Keys:
  - Perplexity AI API key
  - OpenAI API key (GPT-4o access)
  - HeyGen API key
  - YouTube Data API v3 (OAuth2 credentials)

### Setup Steps

1. **Import workflow** into n8n:
   ```
   n8n import:workflow --input=workflows/content-pipeline-main.json
   ```

2. **Configure credentials** in n8n:
   - Add Perplexity API key as HTTP Header Auth
   - Add OpenAI API key
   - Add HeyGen API key
   - Configure YouTube OAuth2

3. **Set topic input** — trigger manually or configure schedule

4. **Run pipeline** — monitor execution in n8n UI

5. **Check YouTube** — video published in ~5-10 minutes

---

## 📊 Pipeline Metrics

| Metric | Manual Process | Automated Pipeline |
|--------|---------------|--------------------|
| Research time | 30-60 min | ~45 seconds |
| Script writing | 45-90 min | ~30 seconds |
| Video creation | 2-4 hours | ~5-8 minutes |
| YouTube upload | 15-30 min | ~2 minutes |
| **Total time** | **3-6 hours** | **~10 minutes** |
| **Cost per video** | $50-200 (human) | ~$2-5 (API costs) |

---

## 💡 Use Cases

- 🤖 **AI & Tech Education** — Weekly videos on LLMs, agents, RAG, cloud
- 💼 **Product Insights** — Product management tips and frameworks
- 🌐 **Industry News** — Daily AI news roundups
- 📊 **Market Analysis** — Stock, tech, and business trend videos
- 🇭🇮 **Hindi/English** — Bilingual content for broader audience reach

---

## 🔧 Advanced Configurations

### Multi-Language Support
- Translate script to Hindi using DeepL/GPT-4o before HeyGen
- Use HeyGen Hindi voice avatar for regional audience

### SEO Optimization
- Add ChatGPT node to generate 15+ YouTube tags
- Auto-generate chapters from script sections
- A/B test thumbnails with YouTube API

### Content Calendar Integration
- Connect to Google Sheets for topic queue
- Auto-schedule based on trending topics from Perplexity
- Track published videos in Notion or Airtable

---

## 📌 About the Author

**Vinit Metange** — AI Product Leader with 18+ years building intelligent platforms.
Currently at Netcracker Technology, leading GenAI platform strategy.

This pipeline is part of **CineVerse Studio** — an AI-powered content creation agency.

**Connect**: [LinkedIn](https://www.linkedin.com/in/vinit-metange) | [GitHub](https://github.com/VinitMetange)

---

*Part of [Vinit Metange's GitHub Portfolio](https://github.com/VinitMetange) | n8n AI Automation Series*
