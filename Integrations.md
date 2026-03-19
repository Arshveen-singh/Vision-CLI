# Vision CLI — Integrations Guide

Opt-in integrations. Nothing installs unless you want it. Every integration follows the same pattern: setup → send → wire into main loop → wire into automations.

---

## Already Built

| Integration | Commands | Status |
|-------------|----------|--------|
| Telegram | `/telegramsetup` `/telegram` `/telegramread` | ✅ |
| Email (SMTP) | `/emailsetup` `/email` | ✅ |
| GitHub | `/ghconnect` `/myrepos` `/repoload` etc. | ✅ |

---

## WhatsApp (Twilio)

```bash
pip install twilio
```

```python
def whatsapp_setup():
    sid    = os.environ.get("TWILIO_SID")   or input("Twilio Account SID: ").strip()
    token  = os.environ.get("TWILIO_TOKEN") or input("Twilio Auth Token: ").strip()
    number = input("Your WhatsApp number (+91...): ").strip()
    data["twilio_sid"]      = sid
    data["twilio_token"]    = token
    data["whatsapp_number"] = number
    save_data()
    console.print("[green]✓ WhatsApp configured[/green]")

def whatsapp_send(message, to=None):
    from twilio.rest import Client
    sid    = data.get("twilio_sid")
    token  = data.get("twilio_token")
    number = data.get("whatsapp_number")
    to     = to or number
    if not all([sid, token, number]):
        console.print("[yellow]Run /whatsappsetup first[/yellow]"); return False
    try:
        client = Client(sid, token)
        client.messages.create(
            body=message,
            from_="whatsapp:+14155238886",  # Twilio sandbox
            to=f"whatsapp:{to}"
        )
        console.print("[green]✓ WhatsApp sent[/green]"); return True
    except Exception as e:
        console.print(f"[red]WhatsApp error: {e}[/red]"); return False
```

**Main loop:**
```python
elif user == "/whatsappsetup":        whatsapp_setup()
elif user.startswith("/whatsapp "): whatsapp_send(user[11:].strip())
```

**Automation:**
```python
elif action.startswith("/whatsapp "): whatsapp_send(action[11:])
```

---

## Discord

```bash
pip install discord.py
```

```python
def discord_setup():
    token      = os.environ.get("DISCORD_TOKEN")   or input("Discord Bot Token: ").strip()
    channel_id = os.environ.get("DISCORD_CHANNEL") or input("Channel ID: ").strip()
    data["discord_token"]   = token
    data["discord_channel"] = channel_id
    save_data()
    console.print("[green]✓ Discord configured[/green]")

def discord_send(message):
    import discord, asyncio
    token      = data.get("discord_token")
    channel_id = data.get("discord_channel")
    if not token or not channel_id:
        console.print("[yellow]Run /discordsetup first[/yellow]"); return False
    async def _send():
        client = discord.Client(intents=discord.Intents.default())
        @client.event
        async def on_ready():
            ch = client.get_channel(int(channel_id))
            await ch.send(message)
            await client.close()
        await client.start(token)
    try:
        asyncio.run(_send())
        console.print("[green]✓ Discord sent[/green]"); return True
    except Exception as e:
        console.print(f"[red]Discord error: {e}[/red]"); return False
```

---

## Google Calendar

```bash
pip install google-api-python-client google-auth-httplib2 google-auth-oauthlib
```

Place `credentials.json` (from console.cloud.google.com) in Vision CLI directory.

```python
def gcal_setup():
    from google_auth_oauthlib.flow import InstalledAppFlow
    from google.auth.transport.requests import Request
    import pickle
    SCOPES = ['https://www.googleapis.com/auth/calendar']
    creds = None
    if os.path.exists('gcal_token.pickle'):
        with open('gcal_token.pickle','rb') as f: creds = pickle.load(f)
    if not creds or not creds.valid:
        if creds and creds.expired and creds.refresh_token:
            creds.refresh(Request())
        else:
            flow = InstalledAppFlow.from_client_secrets_file('credentials.json', SCOPES)
            creds = flow.run_local_server(port=0)
        with open('gcal_token.pickle','wb') as f: pickle.dump(creds, f)
    console.print("[green]✓ Google Calendar connected[/green]")

def gcal_add(title, date_str, time_str="10:00", duration_mins=60):
    from googleapiclient.discovery import build
    from datetime import datetime, timedelta
    import pickle
    with open('gcal_token.pickle','rb') as f: creds = pickle.load(f)
    service = build('calendar','v3',credentials=creds)
    start = datetime.strptime(f"{date_str} {time_str}","%Y-%m-%d %H:%M")
    end   = start + timedelta(minutes=duration_mins)
    event = {
        'summary': title,
        'start': {'dateTime': start.isoformat(), 'timeZone': 'Asia/Kolkata'},
        'end':   {'dateTime': end.isoformat(),   'timeZone': 'Asia/Kolkata'},
    }
    result = service.events().insert(calendarId='primary', body=event).execute()
    console.print(f"[green]✓ Event: {result.get('htmlLink')}[/green]")
```

---

## Notion

```bash
pip install notion-client
```

```python
def notion_setup():
    token = os.environ.get("NOTION_TOKEN") or input("Notion Integration Token: ").strip()
    db_id = input("Database ID (from URL): ").strip()
    data["notion_token"] = token
    data["notion_db"]    = db_id
    save_data()
    console.print("[green]✓ Notion configured[/green]")

def notion_add(title, content=""):
    from notion_client import Client
    token = data.get("notion_token"); db = data.get("notion_db")
    if not token or not db:
        console.print("[yellow]Run /notionsetup first[/yellow]"); return
    try:
        client = Client(auth=token)
        client.pages.create(
            parent={"database_id": db},
            properties={"Name": {"title": [{"text": {"content": title}}]}},
            children=[{"object":"block","type":"paragraph",
                        "paragraph":{"rich_text":[{"type":"text","text":{"content":content}}]}}]
            if content else []
        )
        console.print(f"[green]✓ Added to Notion: {title}[/green]")
    except Exception as e:
        console.print(f"[red]Notion error: {e}[/red]")
```

---

## Spotify

```bash
pip install spotipy
```

```python
def spotify_setup():
    client_id     = os.environ.get("SPOTIFY_CLIENT_ID")     or input("Spotify Client ID: ").strip()
    client_secret = os.environ.get("SPOTIFY_CLIENT_SECRET") or input("Spotify Secret: ").strip()
    data["spotify_id"]     = client_id
    data["spotify_secret"] = client_secret
    save_data()
    console.print("[green]✓ Spotify configured[/green]")

def spotify_play(query):
    import spotipy
    from spotipy.oauth2 import SpotifyOAuth
    sp = spotipy.Spotify(auth_manager=SpotifyOAuth(
        client_id=data.get("spotify_id"),
        client_secret=data.get("spotify_secret"),
        redirect_uri="http://localhost:8080",
        scope="user-modify-playback-state"))
    results = sp.search(q=query, limit=1, type='track')
    tracks  = results['tracks']['items']
    if not tracks: console.print("[red]No results.[/red]"); return
    track = tracks[0]
    sp.start_playback(uris=[track['uri']])
    console.print(f"[green]🎵 Playing: {track['name']} — {track['artists'][0]['name']}[/green]")
```

---

## Adding Your Integration — Checklist

1. `def <service>_setup()` — collect credentials, store in `data`, call `save_data()`
2. `def <service>_send(message)` — check for credentials, wrap in try/except
3. Add `elif` in main loop
4. Add to `show_help()` under Integrations section
5. Add to `_execute_automation()` for scheduler support
6. Update CHANGELOG.md

Open a PR — we'll merge it.
