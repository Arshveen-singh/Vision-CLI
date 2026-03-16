# Contributing to Vision CLI

First off — thanks for even thinking about contributing. Vision CLI is a solo project built by a 14-year-old developer, and any help is genuinely appreciated.

---

## How to Contribute

### 🐛 Report a Bug
1. Open a [GitHub Issue](https://github.com/Arshveen-singh/CLI-project/issues)
2. Use the title format: `[BUG] Short description`
3. Include:
   - What you did
   - What you expected
   - What actually happened
   - Error message (if any)
   - OS and Python version

### 💡 Suggest a Feature
1. Open a [GitHub Issue](https://github.com/Arshveen-singh/CLI-project/issues)
2. Use the title format: `[FEATURE] Short description`
3. Describe the feature and why it would be useful

### 🔧 Submit a Pull Request
1. Fork the repo
2. Create a branch: `git checkout -b feature/your-feature-name`
3. Make your changes
4. Test it works
5. Commit: `git commit -m "Add: your feature description"`
6. Push: `git push origin feature/your-feature-name`
7. Open a Pull Request

---

## Code Style

- Python 3.10+
- Use `rich` for all terminal output — no plain `print()` for UI
- Keep functions small and focused
- Add a comment above each function explaining what it does
- Error handling with `try/except` everywhere — never let the CLI crash

---

## Adding New Commands

Commands follow a simple pattern in the main loop:

```python
elif user.startswith("/yourcommand "):
    arg = user[len("/yourcommand "):]
    # your logic here
    console.print("[green]Result[/green]")
```

And add it to `show_help()`:
```python
[green]/yourcommand <arg>[/green]   — What it does
```

---

## Adding New AI Providers

Add to the provider dict and `setup_provider()` function:

```python
YOUR_MODELS = {
    "1": ("model-id", "Model Name — Description"),
}

# In setup_provider():
elif provider == "4":
    from your_library import YourClient
    key = os.environ.get("YOUR_API_KEY") or input("API key: ").strip()
    return YourClient(api_key=key), "YourProvider", YOUR_MODELS
```

---

## What We Need Help With

- [ ] Windows testing and bug fixes
- [ ] Better stock data (more sectors, global markets)
- [ ] Voice wake word ("Hey Vision")
- [ ] Textual TUI implementation
- [ ] Better image generation models
- [ ] Plugin system for custom commands
- [ ] Unit tests

---

## Contact

**Arshveen Singh** — [Arshveensingh@proton.me](mailto:Arshveensingh@proton.me)

Open an issue first before working on anything big — saves everyone time.
