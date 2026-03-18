# Contributing to Vision CLI

First off — thanks for being here. Vision CLI is a solo-built open source project and every contribution matters.

---

## What we need most

These are the highest-priority open issues. Pick one and go:

### Features (good first contributions)
- **Rolling context summarization** — currently trims at 20 messages. Replace with a rolling summary so no context is ever lost
- **Auto web search in chat** — Vision should auto-search when it doesn't know something, not just via `/search`
- **Wake word detection** — "Hey Vision" triggers mic input from idle state
- **Long context support** — proper chunking + summarization for large documents
- **Flutter mobile app** — companion app for cross-device sync

### Integrations (30–50 lines each)
- WhatsApp (Twilio API)
- Discord (discord.py)
- Google Calendar (google-api-python-client)
- Google Drive (google-api-python-client)
- Notion (notion-client)
- Spotify (spotipy)

See [INTEGRATIONS.md](INTEGRATIONS.md) for exact patterns.

### New providers (10–20 lines each)
Any OpenAI-compatible provider can be added in ~15 lines. See the pattern below.

---

## How to add a new provider

Most AI providers use the OpenAI-compatible API. Adding one takes 15 lines.

**Step 1 — Add to `select_provider()`:**
```python
("11", "NewProvider", "Description of it"),
```

**Step 2 — Add to `setup_provider()`:**
```python
elif provider == "11":
    key = os.environ.get("NEWPROVIDER_API_KEY") or input("NewProvider key: ").strip()
    return OpenAI(base_url="https://api.newprovider.com/v1", api_key=key), "NewProvider"
```

**Step 3 — Add suggested models:**
```python
NEWPROVIDER_SUGGESTED = [
    ("model-id-1", "Model Name 1 — description"),
    ("model-id-2", "Model Name 2 — description"),
]
```

**Step 4 — Add to `_get_suggested()`:**
```python
"NewProvider": (NEWPROVIDER_SUGGESTED, "docs.newprovider.com/models"),
```

That's it. The model selector, validation, council, and everything else works automatically.

---

## How to add a new integration

An integration is any external service (messaging, calendar, etc.).

**Pattern — keep it simple:**

```python
# ── Setup ──
def newservice_setup():
    token = os.environ.get("NEWSERVICE_TOKEN") or input("NewService token: ").strip()
    data["newservice_token"] = token
    save_data()
    console.print("[green]✓ NewService configured[/green]")

# ── Send ──
def newservice_send(message):
    token = data.get("newservice_token")
    if not token:
        console.print("[yellow]Run /newservicesetup first[/yellow]")
        return False
    try:
        # your API call here
        r = requests.post("https://api.newservice.com/send",
                          json={"token": token, "message": message}, timeout=10)
        if r.status_code == 200:
            console.print("[green]✓ Sent[/green]")
            return True
    except Exception as e:
        console.print(f"[red]Error: {e}[/red]")
        return False
```

**Add commands to the main loop:**
```python
elif user == "/newservicesetup":  newservice_setup()
elif user.startswith("/newservice "): newservice_send(user[13:].strip())
```

**Add to `show_help()`:**
```python
[green]/newservicesetup  /newservice <msg>[/green]
```

**Add to automations** (optional, for scheduled sends):
```python
elif action.startswith("/newservice "): newservice_send(action[13:])
```

See [INTEGRATIONS.md](INTEGRATIONS.md) for specific implementation guides per service.

---

## How to add a new command

Any feature that doesn't need a new integration:

1. Write the function (follow existing style — Rich console output, try/except, `save_data()` if it persists anything)
2. Add the `elif user.startswith("/yourcommand"):` block in the main loop, grouped with similar commands
3. Add it to `show_help()` in the right section
4. Update CHANGELOG.md

---

## Code style

- Use Rich for all terminal output — `console.print()`, `Panel()`, `Table()`, `Markdown()`
- Wrap API calls in `try/except` — never let an integration crash the main loop
- Use `save_data()` whenever you modify `memory`, `goals`, `portfolio`, or `automations`
- Rate limit model calls — use `rate_limit(model)` before every `client.chat.completions.create()`
- Keep functions focused — one function, one job
- Comment non-obvious logic inline

---

## Pull request checklist

- [ ] Feature works end-to-end (tested in Colab or local)
- [ ] No crashes on failure (try/except around all external calls)
- [ ] Added to `show_help()`
- [ ] Added to CHANGELOG.md under the right version
- [ ] No hardcoded API keys (use `os.environ.get()` + `input()` fallback)
- [ ] Works with existing `save_data()` / `load_data()` pattern if it stores anything

---

## Open issues to claim

| Issue | Difficulty | What's needed |
|-------|-----------|---------------|
| Rolling context summarization | Medium | Replace 20-message trim with summarize-then-trim |
| Auto web search in chat | Medium | Detect when Vision doesn't know → auto `/search` |
| WhatsApp integration | Easy | Twilio API, 40 lines |
| Discord integration | Easy | discord.py, 50 lines |
| Google Calendar | Medium | OAuth2 flow + event creation |
| Wake word detection | Hard | Background mic thread, hotword model |
| Flutter mobile app | Hard | Cross-device sync via shared JSON/API |
| Self-improving engine | Hard | Usage analytics + auto model selection |

---

## Questions?

Open an issue or start a discussion. Contributions of any size are welcome — even fixing a typo or adding a model to the suggestion list counts.
