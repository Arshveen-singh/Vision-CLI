# Changelog

All notable changes to Vision CLI are documented here.

---

## [3.4.5-beta] — 2026-03-19

### Added — Competitive Features
- **`?<query>` web search shorthand** — instant DuckDuckGo search, one character prefix
  - `?bitcoin price today` → search + synthesized answer with sources
  - Faster than typing `/search`
- **`/session save/load/list`** — full session state persistence
  - Saves: history, advisor history, active skills, CM/AM state, model, rolling summaries
  - `/session save "debugging"` → pickle file with everything
  - `/session load 1` → restores exactly where you left off
- **`VISION.md` project context** — auto-loaded on startup
  - `/vision.md` creates template in current directory
  - Defines: stack, rules, important files, run commands
  - Vision reads it every session automatically (like CLAUDE.md / GEMINI.md)
- **`/cm test <file>`** — pytest auto-fix loop
  - Runs pytest → captures failures → AI fixes code → reruns
  - Up to 3 attempts automatically
  - Closes the biggest gap with Claude Code
- **`/cm multifile <task>`** — generate multiple files simultaneously
  - Plans file structure via AI → generates each file in parallel
  - Creates directories, writes up to 6 files at once
  - e.g. `/cm multifile FastAPI auth system with JWT`
- **`/cm watch [folder]`** — folder watcher (like Cursor background agent)
  - Watches `.py`, `.js`, `.ts` files for changes
  - Auto-reviews on every save — spots bugs, suggests improvements
  - `/cm watch stop` to stop
- **`/cm sandbox <task>`** — isolated code execution
  - Runs code with CPU time limits (10s max)
  - Clean environment — can't damage Colab session
  - Works on existing files or generates + runs
- **`/cmgit log [n]`** — git history + AI explanation
  - Shows last N commits with plain-English summary
  - "What was being built? What changed?"
- **`/cmgit blame <file>`** — git blame + AI context
  - Explains why specific code exists based on commit history

### Added — AutoMode (Vision CLI AM)
- **`/automode` or `/am`** — dedicated automation OS mode
  - Full ASCII banner: `AUTO MODE VISION AM`
  - Purple theme (distinct from CodeMode green)
  - Separate system prompt: thinks in workflows, triggers, chains
- **`/am workflow <desc>`** — natural language → structured automation plan
  - Converts "every morning check RELIANCE and send to telegram" into full plan
  - Shows: trigger, steps chain, Vision CLI commands, monitoring, failure points
  - Save workflow by name
- **`/am chain step1 → step2 → step3`** — sequential task execution
  - Chains: `/stock`, `/weather`, `/telegram`, `/search`, `shell:cmd`
  - Shows live progress per step
- **`/am script <task>`** — generate standalone automation script
  - Complete Python script with logging, error handling, timestamps
  - Ready to schedule with `/automate`
- **`/am monitor <target>`** — set up monitoring + alerts
  - Generates monitoring function + `/automate` command + Telegram alert
- **`/am status`** — show all active automations
- **`/am workflows`** — list saved workflows
- **`/ammem add/view/forget`** — AutoMode-specific memory

### Added — Providers (17 total)
- **OpenAI** — GPT-4o, o1, o1-mini, GPT-4 Turbo
- **Anthropic** — Claude Sonnet 4.5, Opus 4.5, Haiku 4.5
- **Google** — Gemini 2.0 Flash, 2.0 Flash Thinking, 1.5 Pro
- **xAI** — Grok 3, Grok 3 Mini, Grok 2 Vision
- **DeepSeek** — DeepSeek V3 (chat), DeepSeek R1 (reasoner) — cheapest frontier
- **Cohere** — Command R+, Command R, Command Light
- **Perplexity** — Sonar Large/Small (web-grounded)
- Each provider has: setup guide, suggested models, FREE/PAID tags, docs link
- NVIDIA setup now shows step-by-step key generation guide
- Provider selection shows FREE/PAID badges in colored columns

### Fixed — Streaming
- Replaced Rich `Live()` with `sys.stdout.write()` streaming
- Now works properly in Colab AND terminal
- Groq streaming re-enabled (no longer auto-disabled)
- Think tag `<think>...</think>` hidden during stream, stripped from final reply

### Fixed — Unknown Commands
- Fuzzy suggestions: `/ghsetup` → "Did you mean: /ghconnect /myrepos /help"
- CodeMode-aware: different hint message when in CM
- Shows all 80+ known commands in suggestion pool

### Fixed — Weather
- Handles both `current_condition` and `current_conditions` keys
- Falls back to plain text if JSON parse fails
- Never throws KeyError regardless of wttr.in response format

### Fixed — 503 Server Errors
- Actionable error message: "Groq is down. Try llama-3.1-8b-instant or switch provider"
- Auto-retry up to 3x with exponential backoff (3s, 6s, 9s)
- Status page link included: groqstatus.com

---

## [1.4.4-beta] — 2026-03-15

### Added — CodeMode
- `/codemode` or `/cm` — dedicated developer environment
- ASCII banner, coding model selector, API key check on entry
- `CODEMODE_SYSTEM_PROMPT` — separate from skill file
- `/cm plan`, `/cm build`, `/cm run` (auto-fix loop), `/cm review`
- `/cm debug`, `/cm refactor`, `/cm swarm`, `/cm search`
- `/cm swarm-select` — different model per swarm agent
- `/apply`, `/edit` — file editing
- `/cmmem`, `/cmgit status`, `/cmgit diff`, `/cmcommit`
- GitHub: `/reposelect`, `/repoedit`, `/reporeview`
- Input prompt: `[YOU:CM] →`

### Added — Setup Wizard, Undo, Export, API Mode
- First-run setup wizard with provider guide
- `/undo` + `/undo history` — undo last memory/automation/goal
- `/export [label]` — full session → markdown file
- `/api` or `--api` flag → local Flask server on localhost:7842
- Multi-session Council: `/council history`, `view`, `compare`
- Skill marketplace: `/skill marketplace`, `/skill install`

---

## [4.3.0] — 2026-03-12

### Added
- Rolling context summarization — MAX_HISTORY=40, compresses oldest 20
- `/context` — context window status
- `/clear` resets summaries

---

## [4.2.0] — 2026-03-10

### Added
- Skills system — 5 built-in skills, `/skill` commands
- `/refresh` — fix disappearing prompt in Colab
- Identity fix — Vision never leaks underlying model
- Advisor context fix — no stale council verdicts

---

## [4.1.0] — 2026-03-07

### Added
- Self-improving engine — `/selfimprove`, `/economy`, `/weeklyreport`, `/patterns`
- Predictive automation

---

## [4.0.0] — 2026-03-04

### Added
- Multi-agent task engine — `/agent`

---

## [3.9.0] — 2026-02-28

### Added
- Automation scheduler, Telegram, Email

---

## [3.8.0] — 2026-02-24

### Added
- GitHub integration

---

## [3.7.0] — 2026-02-20

### Added
- Real-time streaming, `/vision` image input

---

## [3.6.0] — 2026-02-16

### Added
- Auto-memory, tagged memory

---

## [3.5.0] — 2026-02-12

### Added
- 9 providers, model validation

---

## [3.2.0] — 2026-02-05

### Added
- LLM Council, Debate Mode

---

## [3.0.0] — 2026-01-28

### Added
- Full Python rewrite, Rich UI, persistent storage
