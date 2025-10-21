# 🤖 Windy Civi: Legislative BlueSky Bots

**Automated social media bots that turn legislative data into actionable content**

This repository contains the bot infrastructure that powers automated BlueSky posts for civic organizations, advocacy groups, and elected officials tracking U.S. legislative activity.

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 🎯 The Problem We're Solving

**People don't check legislative websites.** Even engaged citizens don't have time to monitor bills, votes, and hearings across 50 states.

**But people do check social media.** They follow organizations they trust - advocacy groups, elected officials, journalists.

**The solution:** Bring legislative transparency to where people already are. Instead of building another civic app that sits unused, we're integrating legislative data into the feeds people already follow.

### The Pivot

We built a civic engagement app first - and nobody used it. The lesson: **meet people where they already are.** This is that lesson in action.

---

## 🏗️ Architecture Overview

**This repository** is the bot/engagement layer for the Windy Civi ecosystem. It consumes data from the [Windy Civi data pipeline](https://github.com/windy-civi/opencivicdata-blockchain-transformer) and publishes to social media.

**Data Source (Pipeline):**

- 🏛️ [opencivicdata-blockchain-transformer](https://github.com/windy-civi/opencivicdata-blockchain-transformer) - Main pipeline infrastructure
- 📊 [State pipeline repos](https://github.com/windy-civi-pipelines) - USA, IL, TN, TX, etc.
- ✅ Provides: Clean, structured legislative data with incremental updates

**This Repository (Bot Engine):**

- 🤖 Monitors new bills/actions from pipeline repos
- 🎯 Matches legislation to organization topics/keywords
- ✍️ Generates social media posts
- 📱 Publishes to BlueSky (and other platforms)

```
┌─────────────────────────────────────────────┐
│      Data Pipeline (External Repos)         │
│                                             │
│  github.com/windy-civi-pipelines/           │
│    ├── usa-data-pipeline/                   │
│    ├── il-data-pipeline/                    │
│    └── ...                                  │
│                                             │
│  • Clean, versioned legislative data        │
│  • Nightly updates                          │
│  • Full text extraction                     │
└──────────────────┬──────────────────────────┘
                   │
                   │ (read data)
                   ▼
┌─────────────────────────────────────────────┐
│          THIS REPO: Bot Engine              │
│                                             │
│  • Monitor new bills/actions                │
│  • Match to org topics                      │
│  • Generate posts                           │
│  • Schedule publishing                      │
└──────────────────┬──────────────────────────┘
                   │
                   │ (publish)
                   ▼
          ┌────────────────┐
          │    BlueSky     │
          │   (Twitter/X)  │
          │     (etc.)     │
          └────────────────┘
```

---

## 🎭 Use Cases

### Advocacy Organizations

**Black Lives Matter**

- Auto-post when bills about criminal justice reform, police accountability, or voting rights are introduced
- Example: _"🚨 New bill in Illinois: HB1234 would require body cameras on all police officers. Status: Committee hearing scheduled for Jan 15."_

**LGBTQIA+ Groups**

- Track legislation affecting LGBTQ+ rights, healthcare access, discrimination protections
- Example: _"⚠️ Texas SB567 moves to Senate floor: Would restrict gender-affirming care for minors. Call your senator."_

**Environmental Orgs**

- Monitor climate policy, renewable energy funding, conservation bills
- Example: _"🌱 Victory: California AB890 passes House - $2B for solar infrastructure. Senate vote next week."_

### Elected Officials

**State Senators & Representatives**

- Auto-post when they sponsor, co-sponsor, or vote on legislation
- Example: _"Today I voted YES on HB234 - expanding Medicaid to cover vision and dental care for 50,000 Illinois residents."_

**U.S. Congress Members**

- Share federal legislation with constituent-friendly summaries
- Example: _"I'm co-sponsoring HR1010, the Clean Water Act Reauthorization. This bill would..."_

### Researchers & Journalists

**Policy Think Tanks**

- Track legislation in specific domains (education, healthcare, housing)
- Auto-generate analysis threads when major bills are introduced

**News Outlets**

- Get alerts on high-impact bills before they become mainstream news
- Automatically post bill status updates to news account feeds

---

## ✨ Key Features (Planned)

### Phase 1: Basic Bot (MVP) 🚧

- [ ] Connect to BlueSky API
- [ ] Post bill updates on schedule
- [ ] Simple keyword matching (topics → bills)
- [ ] Single account support

### Phase 2: Multi-Org Platform 📋

- [ ] Organization signup/login
- [ ] Configure topics, keywords, bill sponsors
- [ ] Multi-platform support (BlueSky + Twitter/X)
- [ ] Post approval workflow (draft → review → publish)

### Phase 3: Intelligence Layer 🔮

- [ ] AI-powered bill summarization
- [ ] Topic classification (auto-tag bills)
- [ ] Impact analysis ("who's affected, how")
- [ ] Suggested post text generation
- [ ] Sentiment-aware language (urgent vs. celebratory)

### Phase 4: Analytics & Engagement 📊

- [ ] Post performance tracking
- [ ] Engagement analytics
- [ ] Optimal posting time suggestions
- [ ] A/B testing for post formats

---

## 🛠️ Technical Stack

**Language:** Python 3.12+
**Frameworks:** (TBD - FastAPI for API? Celery for scheduled tasks?)
**BlueSky SDK:** [atproto](https://github.com/bluesky-social/atproto) (Python client)
**Data Source:** [Windy Civi Pipeline Repos](https://github.com/windy-civi-pipelines)
**Deployment:** (TBD - GitHub Actions? Cloud Functions? Railway?)

---

## 🚀 Getting Started

### Prerequisites

- Python 3.12+
- BlueSky account with API credentials
- Access to a Windy Civi data pipeline repo (e.g., [usa-data-pipeline](https://github.com/windy-civi-pipelines/usa-data-pipeline))

### Installation

```bash
# Clone the repository
git clone https://github.com/YOUR-ORG/windy-civi-bluesky-bots.git
cd windy-civi-bluesky-bots

# Install dependencies
pip install -r requirements.txt
# or
pipenv install

# Set up environment variables
cp .env.example .env
# Edit .env with your BlueSky credentials
```

### Configuration

```yaml
# config.yml example
bot:
  name: "IL Climate Action Bot"
  bluesky_handle: "@ilclimate.bsky.social"

topics:
  - keywords: ["climate", "renewable energy", "carbon emissions"]
  - bill_sponsors: ["Rep. Smith", "Sen. Johnson"]

posting:
  frequency: "daily" # or "realtime"
  time: "09:00 CT"
  max_posts_per_day: 5

data_source:
  pipeline_repo: "windy-civi-pipelines/il-data-pipeline"
  watch_sessions: ["103", "104"]
```

### Running the Bot

```bash
# Run once (manual)
python bot.py --config config.yml

# Run as scheduled service
python bot.py --config config.yml --daemon

# Dry run (preview posts without publishing)
python bot.py --config config.yml --dry-run
```

---

## 📁 Project Structure (Planned)

```
windy-civi-bluesky-bots/
├── bot/
│   ├── __init__.py
│   ├── bluesky_client.py      # BlueSky API wrapper
│   ├── data_fetcher.py         # Pull from pipeline repos
│   ├── matcher.py              # Match bills to topics
│   ├── post_generator.py       # Generate post text
│   └── scheduler.py            # Handle posting schedule
├── config/
│   ├── config.schema.json      # Config validation
│   └── example_config.yml      # Example configuration
├── tests/
│   ├── test_matcher.py
│   └── test_post_generator.py
├── .env.example
├── requirements.txt
├── README.md
└── bot.py                      # Main entry point
```

---

## 🧪 Testing

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=bot

# Test specific module
pytest tests/test_matcher.py
```

---

## 🤝 Contributing

This is part of the [Windy Civi](https://github.com/windy-civi) ecosystem. Contributions welcome!

**Getting involved:**

1. Check out the [main pipeline repo](https://github.com/windy-civi/opencivicdata-blockchain-transformer)
2. Review open issues
3. Submit PRs or feature requests

**Development philosophy:**

- Start simple, iterate quickly
- Meet users where they are
- Open source, transparent, durable

---

## 🔗 Related Projects

- **[OpenCivicData Blockchain Transformer](https://github.com/windy-civi/opencivicdata-blockchain-transformer)** - Main data pipeline
- **[USA Data Pipeline](https://github.com/windy-civi-pipelines/usa-data-pipeline)** - Federal legislative data
- **[State Pipelines](https://github.com/windy-civi-pipelines)** - All 50 states (expanding)

---

## 📜 License

MIT License - Use freely, contribute openly.

---

## 📫 Contact

- **Main Project:** [Windy Civi](https://github.com/windy-civi)
- **Website:** [windycivi.com](https://windycivi.com)
- **Organization:** Chicago-based civic tech initiative

---

**Meet people where they are. Make legislative transparency automatic.** 🏛️✨

_Part of the [Windy Civi](https://github.com/windy-civi) ecosystem._
