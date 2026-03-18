# Vision CLI v4.0

**Very Intelligent System I Occasionally Need**

A privacy-first, open-source AI terminal agent built by [Arshveen Singh](https://github.com/Arshveen-singh). Run any LLM from your terminal — with multi-model council debates, GitHub integration, automation scheduling, Telegram, smart memory, and more.

```
██╗   ██╗██╗███████╗██╗ ██████╗ ███╗   ██╗     ██████╗██╗     ██╗
██║   ██║██║██╔════╝██║██╔═══██╗████╗  ██║    ██╔════╝██║     ██║
██║   ██║██║███████╗██║██║   ██║██╔██╗ ██║    ██║     ██║     ██║
╚██╗ ██╔╝██║╚════██║██║██║   ██║██║╚██╗██║    ██║     ██║     ██║
 ╚████╔╝ ██║███████║██║╚██████╔╝██║ ╚████║    ╚██████╗███████╗██║
  ╚═══╝  ╚═╝╚══════╝╚═╝ ╚═════╝ ╚═╝  ╚═══╝     ╚═════╝╚══════╝╚═╝
```

---

## What is Vision CLI?

Vision CLI is a Python-based terminal AI agent that:

- Connects to **9 AI providers** (Groq, OpenRouter, Ollama, Together, Fireworks, Mistral, Cerebras, NVIDIA NIM, SambaNova)
- Runs an **LLM Council** — multiple models answer in parallel, a Chairman synthesizes the verdict
- Has a **Debate Mode** — models argue FOR/AGAINST on any motion
- Tracks **persistent memory** across sessions, tagged and auto-extracted
- Integrates with **GitHub** — load repos, ask questions, multi-model code review
- Schedules **automations** — daily news, portfolio checks, Telegram alerts
- **Streams responses** in real-time
- Plays **music** from YouTube via yt-dlp
- Understands **images** via vision-capable models
- Has a separate **Advisor mode** — brutally honest personal advisor with goal tracking
- Is **100% open source** and runs locally or on Google Colab

---

## Quickstart (Google Colab)

```python
# Install dependencies
!pip install openai groq rich yfinance duckduckgo-search requests \
             beautifulsoup4 wikipedia PyGithub

# Run
!python vision_cli_v4.py
```

Set your API key as an environment variable before running:

```python
import os
os.environ["OPENROUTER_API_KEY"] = "sk-or-..."   # OpenRouter (recommended)
os.environ["GROQ_API_KEY"] = "gsk_..."            # Groq (free, ultra fast)
```

---

## Quickstart (Local)

```bash
# Clone
git clone https://github.com/Arshveen-singh/Vision-CLI
cd Vision-CLI

# Install
pip install -r requirements.txt

# Run
python vision_cli_v4.py
```

---

## Requirements

```
openai>=1.0.0
groq
rich
yfinance
duckduckgo-search
requests
beautifulsoup4
wikipedia
PyGithub

# Optional (install for specific features)
yt-dlp          # /play music
pygame          # music playback
pyttsx3         # TTS /mic
SpeechRecognition # voice input
easyocr         # /ocr
playwright      # /browse
```

---

## Features

### 9 AI Providers

| # | Provider | Free Tier | Speed | Best For |
|---|----------|-----------|-------|----------|
| 1 | **Groq** | ✅ Generous | ⚡ Ultra fast | Daily use |
| 2 | **OpenRouter** | ✅ Some models | 🔥 Varies | Access any model |
| 3 | **Ollama** | ✅ Fully free | 🖥️ Local | Privacy, offline |
| 4 | **Together AI** | ✅ Free tier | ⚡ Fast | Open source models |
| 5 | **Fireworks AI** | ✅ Free tier | ⚡ Fast | Inference speed |
| 6 | **Mistral AI** | ❌ Paid | 🔥 Fast | Official Mistral |
| 7 | **Cerebras** | ✅ Free tier | ⚡⚡ Groq-speed | LLaMA models |
| 8 | **NVIDIA NIM** | ✅ Free credits | 🔥 Fast | 405B Llama |
| 9 | **SambaNova** | ✅ Free | 🔥 Fast | Free 405B Llama |
| 10 | **Bytez** | ✅ Free tier | 🔄 Variable | 175k+ HuggingFace models |

---

### LLM Council ⚖

The flagship feature. Ask a question and get multiple AI models answering independently — then a Chairman synthesizes a final verdict.

```
/council is learning to code more valuable than a degree in 2026?

→ Grok 4.20:       "Depends on the field..."
→ Gemini 2.0:      "Coding is table stakes..."
→ DeepSeek R1:     "The credential still matters..."
→ LLaMA 3.3:       "Hybrid path is optimal..."

⚖ Chairman (Claude Opus): [Final synthesized verdict]
```

**Debate Mode** — models argue assigned positions:

```
/debate AI will replace software engineers within 10 years

→ Grok:      FOR  — argues the case
→ Gemini:    AGAINST — argues the counter
→ DeepSeek:  SKEPTIC — challenges both
→ Chairman:  Judges + gives real answer
```

---

### Smart Memory

Vision automatically remembers things about you across sessions:

```
/memory add name Arshveen #personal
/memory add project Vision CLI #personal
/memory view
/memory view #stock
/memory forget name
```

Auto-memory runs silently in the background after every conversation — it extracts facts worth saving without you having to ask.

---

### GitHub Integration

```bash
/ghconnect                      # Connect your GitHub account
/myrepos                        # List your repos
/repoload Arshveen-singh/Vision-CLI   # Load repo into context
/repofile src/main.py           # Read a specific file
/repoask how does the auth work?  # Ask about the loaded repo
/reporeview                     # Council reviews the codebase
/commit "feat: add streaming"   # Stage all, commit, push
```

Council + GitHub = multi-model code review with a chairman verdict. No other tool does this.

---

### Automation Scheduler

```bash
# Daily morning market news
/automate daily:09:00 | /marketnews | Morning market news

# Every 30 minutes portfolio check
/automate interval:30m | /portfolio view | Portfolio check

# Send Telegram alert every 2 hours
/automate interval:2h | chat:What's the latest AI news? | AI news alert

/automations          # List all automations
/autodelete 1         # Remove automation #1
```

---

### Stocks

```bash
/stock RELIANCE          # Live price, P/E, 52-week range
/stock AAPL              # Works for US stocks too
/stocks banking          # Entire sector overview (NSE)
/recommend growth stocks for 2026
/impact Russia-Ukraine escalation   # War → market impact
/portfolio add TCS 10 3800          # Add to portfolio
/portfolio view                     # Live P&L
/marketnews                         # Latest market headlines
```

Indian sectors supported: `banking`, `it`, `pharma`, `auto`, `tata`, `energy`, `fmcg`, `adani`, `smallcap`

---

### All Commands

```
── AI ──────────────────────────────────────────────
/model          Switch model
/provider       Switch provider
/clear          Clear conversation history
/stream         Toggle streaming on/off

── Memory ──────────────────────────────────────────
/memory add <key> <value> [#tag]
/memory view [#tag]
/memory forget <key>

── Chats ───────────────────────────────────────────
/chats save <name>
/chats list
/chats load <#>

── Music 🎵 ─────────────────────────────────────────
/play <song or artist>
/pause  /resume  /stop  /skip
/queue <song>
/nowplaying
/volume <0-100>

── Voice ───────────────────────────────────────────
/mic on   /mic off

── Timer ───────────────────────────────────────────
/timer <minutes>
/stopwatch start/stop/lap/check

── Image ───────────────────────────────────────────
/imagine <prompt>             Free image gen (Pollinations)
/vision <image_path> [question]   Image understanding

── Advisor ─────────────────────────────────────────
/advisor <message>
/goal add <goal>
/goal list
/goal done <#>

── Council ⚖ ────────────────────────────────────────
/council <query>
/debate <motion>
/councilsetup

── Multi-Agent 🤖 ───────────────────────────────────
/agent <complex task>

── GitHub 📁 ────────────────────────────────────────
/ghconnect
/myrepos
/repoload <user/repo>
/repofile <path>
/repoask <question>
/reporeview
/commit <message>

── Integrations 🔗 ──────────────────────────────────
/telegramsetup
/telegram <message>
/telegramread
/emailsetup
/email <to> | <subject> | <body>

── Automation ⚡ ─────────────────────────────────────
/automate <trigger> | <action> | <description>
/automations
/autodelete <#>

── Stocks ───────────────────────────────────────────
/stock <SYMBOL>
/stocks <sector>
/recommend <query>
/impact <event>
/portfolio add <SYM> <qty> <buy_price>
/portfolio view
/portfolio remove <SYM>
/marketnews [query]

── Code ─────────────────────────────────────────────
/code <filename.py> <what to build>
/html <filename.html> <what to build>
/doc <filename.md> <what to write>
/runfile <filename>
/debug <filename>
/git <git command>

── Tools ────────────────────────────────────────────
/search <query>
/scrape <url>
/browse <url>
/wiki <topic>
/weather <city>
/ocr <image_path>
/artifact <name>

── System ───────────────────────────────────────────
/help
/exit  /q  /quit
```

---

## Roadmap

| Version | Status | Features |
|---------|--------|----------|
| v3.5 | ✅ Done | 9 providers (Groq, OpenRouter, Ollama, Together, Fireworks, Mistral, Cerebras, NVIDIA, SambaNova) |
| v3.6 | ✅ Done | Smart auto-memory, tagged memory, persistent storage |
| v3.7 | ✅ Done | Real-time streaming, vision/image input |
| v3.8 | ✅ Done | GitHub integration (read, ask, council review, commit) |
| v3.9 | ✅ Done | Telegram, Email, Automation scheduler |
| v4.0 | ✅ Done | Multi-agent task engine, parallel sub-agents |
| v4.1 | 🔲 Open | Self-improving, wake word, Flutter app, predictive automation |

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) — adding a new provider or integration takes about 30 lines.

See [INTEGRATIONS.md](INTEGRATIONS.md) for instructions on adding WhatsApp, Discord, Google Calendar, Notion, Spotify, and more.

---

## License

MIT — free forever. See [LICENSE](LICENSE).

Built by **Arshveen Singh** • Delhi, India
