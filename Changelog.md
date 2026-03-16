# Changelog

All notable changes to Vision CLI are documented here.

---

## [v3.0.0] — 2026-03-16

### 🧠 Memory System
- Persistent memory across ALL modes (chat, advisor, finance, goals, portfolio)
- Memory saved to `vision_data.json` — survives session restarts
- `/memory add`, `/memory view`, `/memory forget` commands
- Memory context injected into every AI call automatically

### 💬 Chat Library
- Auto-save conversations to `vision_chats/` folder
- `/chats save <name>`, `/chats list`, `/chats load <#>` commands
- Advisor history persists across sessions

### 🎨 Mascot & UI
- Vision CLI TUI prototype designed (HTML/CSS)
- War-room aesthetic — teal/black, ASCII banner, pixel sidebar
- Thinking animation dots in TUI

### 📈 Stocks — Major Overhaul
- Fixed US stock support — AAPL, TSLA, MSFT etc. now work
- Tries NSE → BSE → US (no suffix) automatically
- Currency auto-detects (₹ for Indian, $ for US)
- Advisor now sees recent main chat context

### 🤖 AI Improvements
- Fixed hallucination — history trimmed to last 20 messages
- Removed rude one-liner replies — new system prompt enforces warmth
- `<think>` tags stripped from Qwen reasoning output
- Rate limiting added with countdown for all models
- Better personality across all 3 models

### 🎵 Music Player
- `/play <song>` — streams any song via YouTube (yt-dlp + pygame)

### 🎤 Voice
- `/mic on` / `/mic off` toggle
- TTS output when mic mode is active
- Graceful fallback on Colab (mic unavailable message)

### ⏱ Timer & Stopwatch
- `/timer <minutes>` — study countdown
- `/stopwatch start/stop/lap/check` — full stopwatch

### 🖼 Image Generation
- Pollinations.ai (free, no key) with HuggingFace SD2.1 fallback
- Auto-displays image in Colab

### 🔧 Provider System
- Groq, OpenRouter, Ollama all supported
- `/provider` command to switch mid-session
- `/model` command to switch model anytime

---

## [v2.0.0] — 2026-03-16

### Core Features
- Full AI CLI with model selector (Kimi K2, Qwen 3 32B, LLaMA 3.3 70B)
- Web search via DuckDuckGo (no API key)
- Wikipedia lookup
- Weather widget (wttr.in)
- Web scraping + headless browser (Playwright)
- OCR — extract text from images (EasyOCR)
- Artifact maker — saves AI replies as `.py`, `.html`, `.md`
- Code generation + auto-save
- HTML generation
- Markdown doc generation
- Run Python files inline
- AI debug/fix files
- Git integration
- Indian stock market dashboard (NSE/BSE)
- Portfolio tracker with live P&L
- Personal advisor mode
- Goal tracker
- Market news
- War/event market impact analysis

---

## [v1.0.0] — 2026-03-16

### Initial Release
- Basic AI chat with Groq API
- Model selector (3 models)
- Web search
- File read/write
- Inline Python execution
- Rich terminal UI with panels and markdown
- Vision CLI ASCII banner
