Changelog
All notable changes to Vision CLI are documented here.

[v3.4.0] — 2026-03-17
🔧 Bug Fixes

Fixed model validation false-rejecting valid models — bumped max_tokens from 1 to 10 in test call
Unknown validation errors (provider quirks, parameter issues) now show a yellow warning but pass — real errors surface on actual use, not at selection
Fixed NoneType crash when a subordinate model returns empty content — now gracefully skipped with warning
Fixed chairman 402 error on free OpenRouter tier — reduced chairman max_tokens from 1536 to 800
Added null-check on chairman response before passing to strip_think()

🚪 Exit Commands

Added /q and /quit as instant exit aliases alongside /exit
Cleaner exit in Google Colab where Ctrl+C interrupts the kernel

📋 Model List

Fixed moonshotai/kimi-k2-instruct-0905 → moonshotai/kimi-k2 (correct OpenRouter ID)
Removed broken google/gemini-flash-1.5 → replaced with google/gemini-2.0-flash-001
Added openai/gpt-4o, openai/gpt-5.3-chat, x-ai/grok-4.20-multi-agent-beta, inception/mercury-2


[v3.3.0] — 2026-03-17
🔧 Model Validation

Validation test call bumped to max_tokens=10 — some models reject max_tokens=1
Unknown errors now treated as pass-through (warns but accepts model)

📋 Model List Updates

Corrected several broken OpenRouter model IDs
Added newer frontier models to suggestion list

🚪 Quick Exit

/q command added for fast exit (especially useful in Colab)


[v3.2.0] — 2026-03-17
⚖ LLM Council — New Feature

/council <query> — fires multiple models in parallel, chairman synthesizes a final verdict
/debate <motion> — models are assigned positions (FOR / AGAINST / SKEPTIC / DEVIL'S ADVOCATE) and argue them, chairman judges the winner
/councilsetup — configure or reconfigure chairman + subordinates anytime
Auto-triggers setup on first /council or /debate if not yet configured
Parallel subordinate calls via threading — all models fire simultaneously
Chairman reads all responses and delivers structured verdict: Consensus, Key Conflict, Final Answer, Confidence
Council state resets automatically on /provider switch
3 new system prompts: COUNCIL_SUBORDINATE_PROMPT, COUNCIL_CHAIRMAN_PROMPT, COUNCIL_DEBATE_PROMPT

🤖 Custom Model Selector — Full Rewrite

Replaced hardcoded model dicts with a free-input selector for both main session and council
select_model_main() — startup + /model command. Shows suggestions, accepts any custom model ID
select_model_council() — dedicated council flow: chairman first, then subordinates one by one
validate_model() — fires a 1-token test call before accepting any model. Bad ID = instant ✗ Wrong model name error with retry loop
Duplicate subordinate prevention
Minimum 2, maximum 4 subordinates enforced
setup_provider() simplified — no longer returns model dict, just (client, provider_name)
Model name now shown in help panel footer

📋 Updated Suggestion Lists

GROQ_SUGGESTED — 5 curated Groq models
OPENROUTER_SUGGESTED — 12 curated OpenRouter models with correct IDs
OLLAMA_SUGGESTED — 6 local Ollama models
All lists are reference only — user can type any model ID directly

⏱ Rate Limiting

DeepSeek R1 interval bumped from 3s to 5s — reasoning models need more breathing room


[v3.1.0] — 2026-03-16
🔧 Bug Fixes & Patches

Fixed asyncio.get_event_loop() deprecation in Python 3.10+ — browser now uses asyncio.run()
Fixed save_data() trimming advisor_history in-place causing drift between memory and saved file
Timer now runs in a background thread — CLI stays responsive while timer ticks
Fixed advisor context injection — last 6 main chat messages now actually injected (README claimed this but it wasn't implemented)
Fixed ask() missing memory context — /recommend, /impact etc. now know who they're talking to
Lazy imports enforced — easyocr, wikipedia, yfinance, playwright load only when needed
<think> tags stripped from all model outputs (Qwen reasoning models)
Rate limiting added with countdown for all models
Hallucination reduction — history trimmed to last 20 messages


[v3.0.0] — 2026-03-16
🧠 Memory System

Persistent memory across ALL modes (chat, advisor, finance, goals, portfolio)
Memory saved to vision_data.json — survives session restarts
/memory add, /memory view, /memory forget commands
Memory context injected into every AI call automatically

💬 Chat Library

Auto-save conversations to vision_chats/ folder
/chats save <name>, /chats list, /chats load <#> commands
Advisor history persists across sessions

📈 Stocks — Major Overhaul

Fixed US stock support — AAPL, TSLA, MSFT etc. now work
Tries NSE → BSE → US (no suffix) automatically
Currency auto-detects (₹ for Indian, $ for US)
Advisor now sees recent main chat context

🤖 AI Improvements

Fixed hallucination — history trimmed to last 20 messages
Removed rude one-liner replies — new system prompt enforces warmth
<think> tags stripped from Qwen reasoning output
Rate limiting added with countdown for all models
Better personality across all 3 models

🎵 Music Player

/play <song> — streams any song via YouTube (yt-dlp + pygame)

🎤 Voice

/mic on / /mic off toggle
TTS output when mic mode is active
Graceful fallback on Colab (mic unavailable message)

⏱ Timer & Stopwatch

/timer <minutes> — study countdown
/stopwatch start/stop/lap/check — full stopwatch

🖼 Image Generation

Pollinations.ai (free, no key) with HuggingFace SD2.1 fallback
Auto-displays image in Colab

🔧 Provider System

Groq, OpenRouter, Ollama all supported
/provider command to switch mid-session
/model command to switch model anytime


[v2.0.0] — 2026-03-16
Core Features

Full AI CLI with model selector (Kimi K2, Qwen 3 32B, LLaMA 3.3 70B)
Web search via DuckDuckGo (no API key)
Wikipedia lookup
Weather widget (wttr.in)
Web scraping + headless browser (Playwright)
OCR — extract text from images (EasyOCR)
Artifact maker — saves AI replies as .py, .html, .md
Code generation + auto-save
HTML generation
Markdown doc generation
Run Python files inline
AI debug/fix files
Git integration
Indian stock market dashboard (NSE/BSE)
Portfolio tracker with live P&L
Personal advisor mode
Goal tracker
Market news
War/event market impact analysis


[v1.0.0] — 2026-03-16
Initial Release

Basic AI chat with Groq API
Model selector (3 models)
Web search
File read/write
Inline Python execution
Rich terminal UI with panels and markdown
Vision CLI ASCII banner
