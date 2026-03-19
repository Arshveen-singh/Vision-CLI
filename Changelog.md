## Vision CLI — Changelog

v1.4.4-beta — CodeMode Complete
Full developer environment — system prompt, GitHub integration, file editing, agent swarm
CodeMode System Prompt

CODEMODE_SYSTEM_PROMPT — dedicated system prompt separate from CODEMODE_SKILL

Two-layer architecture: system prompt (identity + rules) + skill file (context injection)
Absolute rules: no pseudocode, error handling always, inline comments always
GitHub awareness: references actual file paths + function names from loaded repo
Security mindset: silently checks every code block for hardcoded secrets, injection vectors, unvalidated input
Structured response format enforced: root cause → code → run → test → next
Identity: "I'm Vision CM" — never leaks underlying model



GitHub Repo Selector

/reposelect — interactive numbered repo picker
/repoload (no args) — same as reposelect
Shows: repo name, language, stars, last updated, loaded status (✅)
Pick by number or type user/repo directly
Auto-saves loaded repo to CM memory when in CodeMode

/repoedit — AI file editing from GitHub

Usage: /repoedit <path> | <instruction>
Fetches file from loaded repo, AI edits in-place
Shows +N / -N line diff after editing
Three options: y save locally / p save + push / n cancel
Auto-generates conventional commit message on push via cm_smart_commit()

/repoask Improvements

Now uses CODEMODE_SYSTEM_PROMPT when in CodeMode, SYSTEM_PROMPT otherwise
Increased context window to 14,000 chars (was 12,000)
References actual file paths in answers
Panel color: green in CodeMode, cyan in normal mode

CodeMode Sub-modes

/cm plan <task> — architecture diagram + file tree + numbered steps + risks
/cm build <task> — direct production code generation
/cm review <file> — deep review with Critical/High/Medium/Low severity ratings
/cm debug <file> — smart debug with root cause explanation
/cm refactor <file> — AI refactors file in-place
/cm swarm <task> — 5 specialist agents in parallel (Architect, Security, Reviewer, Optimizer, TestWriter)
/cm swarm-select <task> — pick different model per swarm agent
/cm search <query> — coding-specific DuckDuckGo search

CodeMode File Editing

/apply — write last generated code directly to disk
/apply <filename> — write to specific file
/edit <file> <instruction> — AI edits existing local file in-place

CM Memory

/cmmem add/view/forget — CodeMode-specific memory separate from Vision memory
Auto-saves loaded_repo and last_plan to CM memory
Injected into all CM prompts via get_cm_memory_context()

CM GitHub Commands

/cmgit status — git status + AI commit message suggestion
/cmgit diff — git diff + AI explanation of every change
/cmcommit — auto-generate conventional commit message + push
/cmcommit "msg" — manual message + push

CM Web Search

/cmsearch <query> — coding-specific search, synthesized answer with code examples
Auto-triggers in CodeMode on queries containing: "how to", "docs for", "npm", "pip", "library for", "install", etc.
Normal chat in CodeMode gets auto-search instead of uncertainty fallback

Agent Swarm

5 specialist roles: Architect, Security, Reviewer, Optimizer, TestWriter
All run in parallel threads simultaneously
Each agent can use a different model via /cm swarm-select
Coordinator (current model) merges all results into final verdict
Saves result to CM memory

Prompt Changes

Replaced: All CodeMode system positions now use CODEMODE_SYSTEM_PROMPT (not CODEMODE_SKILL)
CODEMODE_SKILL remains as skill file content only (injected via active_skills)
Main chat in CodeMode uses green border panel instead of blue


v4.4 — Setup Wizard + 8 Major Features

Setup wizard, actionable errors, data cleanup, /export, auto web search
/undo + /undo history, multi-session council history
Skill marketplace, local API mode (/api, --api flag)


v4.3 — Bigger Context Window

Rolling summarization — MAX_HISTORY=40, compress oldest 20 into summary
/context command, /clear resets summaries


v4.2 — Skills System + Stability

5 built-in skills, /skill commands, /refresh, identity fix, advisor context fix


v4.1 — Self-Improving Engine

/selfimprove, /economy, /weeklyreport, /patterns, predictive automation


v4.0 — Multi-Agent Task Engine

/agent, parallel sub-agents, coordinator merge


v3.9 — Automation + Integrations

Scheduler, open:url, shell:cmd, Telegram, Email


v3.8 — GitHub Integration

/ghconnect, /repoload, /repoask, /reporeview, /commit


v3.7 — Streaming + Vision Input

Rich Live streaming, /vision, Groq auto-disable on Colab


v3.6 — Smart Memory

Auto-memory background thread, tagged memory, injected into all calls


v3.5 — 9 Providers

Together, Fireworks, Mistral, Cerebras, NVIDIA NIM, SambaNova, Bytez


v3.2 — LLM Council

Parallel subordinates + Chairman, Debate Mode, custom model selector


v3.0 — Core Rewrite

Python rewrite, Rich UI, persistent storage, full feature set

The complete developer mode

CodeMode — /codemode or /cm activates a dedicated developer environment

Full ASCII art banner on entry: CODEMODE CLI VISION CM
Coding-specific model selector — every provider shows ranked coding models with Free/Paid tags
API key setup wizard on first enter — guides to get key for selected provider
Injects CODEMODE_SKILL — full detailed skill: production code only, error handling always,
GitHub-aware responses, security severity ratings, ASCII architecture diagrams
Input prompt changes from [YOU] → to [YOU:CM] →
/codemode when already active shows CM-specific command reference
/codemode off to exit back to default Vision mode
Selected coding model replaces current model for the session
Model rankings per provider: Kimi K2, Qwen 3, DeepSeek R1, Codestral, LLaMA 405B etc.




v4.4 — Setup Wizard + 8 Major Features

Setup wizard — first-run only, guides through provider + key + feature overview
Actionable errors — every API failure gives specific fix suggestion (not raw traceback)
Data cleanup — vision_data.json auto-archives at 5MB, trims aggressively after
/export [label] — full session → clean markdown file (chat, council, memories, goals)
Auto web search — detects uncertainty phrases → auto DuckDuckGo → enhanced answer
/undo + /undo history — undo stack for memory, automation, goal adds (last 10)
Multi-session Council history — /council history, view <#>, compare <#> <#>
Skill marketplace — /skill marketplace + /skill install <n> from GitHub
Local API mode — /api or --api flag → Flask on localhost:7842

Endpoints: /chat, /advisor, /memory, /stock/<SYM>, /status
100% local — no cloud, no external server




v4.3 — Bigger Context Window
Rolling summarization — no context ever lost

MAX_HISTORY = 40 — double old limit before compression
Oldest 20 messages compressed into ~200 word summary block
Last 20 always verbatim. Compression in background thread
Works independently for main chat AND advisor
conversation_summary + advisor_summary globals
/context — shows window status, summary size, compression settings
/clear resets everything — history, advisor, summaries, council/agent context


v4.2 — Skills System + Stability

Skills System — vision_skills/ directory with .md skill files

5 built-in skills: coding, security, research, teacher, jarvis
/skill load/unload/list/active/clear/create/edit/reload
Stack multiple skills simultaneously


/refresh — redraws input box, fixes disappearing prompt in Colab
Identity fix — Vision always identifies as Vision, never leaks underlying model
Advisor context fix — /clear resets stale council context from previous sessions
Bytez HTML error fix — hard-rejects HTML error pages instead of warning + passing
Bytez URL fix — corrected endpoint
Bytez warning — displays routing notice on provider selection


v4.1 — Self-Improving Engine

/selfimprove — usage pattern analysis + automation suggestions + model optimization
/economy — sessions, total time, top commands, peak hours dashboard
/weeklyreport — AI-generated productivity report
/patterns — learned predictive automations
predictive_check() — startup trigger for time-based patterns
Session duration tracked, economy updated on exit


v4.0 — Multi-Agent Task Engine

/agent <task> — decomposes into 2-4 specialist roles, parallel execution
Coordinator merges all results into final answer
last_agent_result injected into Vision + Advisor context


v3.9 — Automation + Integrations

Scheduler — daily:HH:MM, interval:Nm/Nh
open:url, shell:cmd, chat:prompt action types
Telegram — /telegramsetup, /telegram, /telegramread
Email (SMTP) — /emailsetup, /email


v3.8 — GitHub Integration

/ghconnect, /myrepos, /repoload, /repofile, /repoask, /reporeview, /commit


v3.7 — Streaming + Vision Input

Real-time streaming via Rich Live
Groq streaming auto-disabled on Colab
/vision <image_path> — image understanding


v3.6 — Smart Memory

Auto-memory — extracts facts silently in background
Tagged memory — #personal, #stock, #weather, #council
Memory injected into all AI calls


v3.5 — 9 Providers

Together, Fireworks, Mistral, Cerebras, NVIDIA NIM, SambaNova, Bytez
get_max_tokens() — Groq auto-caps at 1024
/q and /quit as exit aliases


v3.2 — LLM Council

Parallel subordinate calls + Chairman synthesis
Debate Mode — FOR/AGAINST/SKEPTIC/DEVIL'S ADVOCATE
Custom model selector with validate_model()


v3.0 — Core Rewrite

Python rewrite, Rich UI, persistent storage
Memory, goals, portfolio, advisor, music, timer, image gen, stocks, OCR


## v1.0–v2.x — Early Versions
- Basic CLI chat with Groq
- Initial stock and advisor features
- Vision CLI name established
