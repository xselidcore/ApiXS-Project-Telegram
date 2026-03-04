# ApiXS — Telegram Userbot

**ApiXS** is a self-hosted Telegram userbot built on [Telethon](https://github.com/LonamiWebs/Telethon).
It delivers a rich command suite — AI chat, media downloads, online monitoring, anti-delete, reminders
and more — accessible via a customizable prefix directly in any chat.

> Public demo of the source code.
> Live instance: **https://YOUR_DOMAIN** · Bot: **@YOUR_BOT**

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🤖 **AI Chat** | Multi-turn conversations via Groq LLaMA 3 with per-chat memory |
| 🎙 **Voice Transcription** | Whisper-powered transcription of voice messages and audio |
| ⬇️ **Video Downloader** | TikTok · YouTube · Instagram · Twitter/X · VK · Rutube and more |
| 🚫 **Anti-Delete** | Captures deleted/edited messages, stores in DB, notifies via bot |
| 👁 **Online Monitor** | One-shot and continuous presence tracking with timestamped logs |
| 🎵 **Yandex Music** | "Now playing" card with album art and progress bar |
| 📝 **Notes** | Per-user persistent note storage with search |
| 📋 **Templates** | Reusable text snippets with quick-access shortcuts |
| ⏰ **Reminders** | Time-based reminders delivered to your DM |
| 🔗 **URL Shortener** | Shorten links via is.gd / TinyURL |
| 📱 **QR Code** | Generate QR codes from any text or URL |
| 🌤 **Weather** | Live weather for any city via wttr.in |
| 🔄 **Translate** | Text translation via MyMemory API |
| 📌 **Pin Manager** | Pin/unpin messages silently or with notifications |
| 🔔 **Custom Prefix** | Every user sets their own command prefix |
| 🏷 **Aliases** | Personal command shortcuts |
| 🌐 **Web Dashboard** | Self-hosted status page and user control panel |
| 🔐 **Role System** | Admin / Moderator / User roles with web management |

---

## 🛠 Stack

- **Python 3.11+**
- **[Telethon](https://github.com/LonamiWebs/Telethon)** — Telegram MTProto client
- **[Flask](https://flask.palletsprojects.com/)** — web dashboard & REST API
- **MySQL** — primary data store
- **[Groq API](https://groq.com/)** — LLaMA 3 inference (free tier available)
- **[yt-dlp](https://github.com/yt-dlp/yt-dlp)** — video downloads
- **[Pillow](https://pillow.readthedocs.io/)** — album art card generation
- **[Cryptography](https://cryptography.io/)** — Fernet session encryption

---

## 📦 Installation

### 1. Clone

```bash
git clone https://github.com/YOUR_USERNAME/apixs.git
cd apixs
```

### 2. Virtual environment

```bash
python -m venv .venv
source .venv/bin/activate       # Windows: .venv\Scripts\activate
```

### 3. Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure

```bash
cp .env.example .env
# Edit .env with your credentials (see Configuration section below)
```

### 5. Generate session encryption key

```python
from cryptography.fernet import Fernet
print(Fernet.generate_key().decode())
# Paste the output into SESSION_ENC_KEY in .env
```

### 6. Authenticate Telegram

```bash
python -m ApiXS.session_gen
# Follow the prompts — saves a StringSession to the database
```

### 7. Run

```bash
python ApiXS_start.py
```

---

## ⚙️ Configuration

All settings via environment variables (`.env` file):

| Variable | Required | Description |
|----------|:--------:|-------------|
| `TELEGRAM_API_ID` | ✅ | From [my.telegram.org](https://my.telegram.org) |
| `TELEGRAM_API_HASH` | ✅ | From [my.telegram.org](https://my.telegram.org) |
| `TELEGRAM_BOT_TOKEN` | — | [@BotFather](https://t.me/BotFather) token (for notifications) |
| `DATABASE_URL` | ✅ | `mysql://user:pass@host:port/db` |
| `SESSION_ENC_KEY` | ✅ | Fernet key — `python -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())"` |
| `GROQ_API_KEY` | ✅ | Free key at [console.groq.com](https://console.groq.com) |
| `GROQ_MODEL` | — | Default: `llama-3.3-70b-versatile` |
| `COMMAND_PREFIX` | — | Default: `.axs` |
| `ALLOWED_USERS` | — | Comma-separated Telegram user IDs |
| `STATUS_EXTERNAL_URL` | — | Public URL of your web dashboard |
| `YANDEX_MUSIC_TOKEN` | — | Yandex Music OAuth token |
| `ANTI_DELETE_MEDIA_CHANNEL` | — | Channel ID for media backups |
| `DL_COOKIES_B64` | — | Base64 cookies.txt for YouTube bot-check bypass |
| `AD_DATABASE_URL` | — | Separate DB for anti-delete (falls back to `DATABASE_URL`) |

---

## 📋 Commands

| Command | Shortcut | Description |
|---------|----------|-------------|
| `.axs ai <text>` | `.ai` | Chat with LLaMA 3 |
| `.axs v` | `.v` | Transcribe voice message |
| `.axs dl <url>` | `.dl` | Download video |
| `.axs ad` | `.ad` | Toggle anti-delete |
| `.axs online <user>` | — | Notify when user is online |
| `.axs track <user>` | `.tk` | Continuous presence log |
| `.axs seen <user>` | — | Last seen time |
| `.axs ym np` | `.ynow` | Yandex Music now playing |
| `.axs n <text>` | `.n` | Add note |
| `.axs te <name>` | `.te` | Use template |
| `.axs remind <time>` | `.r` | Set reminder |
| `.axs weather <city>` | `.w` | Weather info |
| `.axs translate <lang>` | `.tr` | Translate text |
| `.axs url short <url>` | — | Shorten URL |
| `.axs qr <text>` | — | Generate QR |
| `.axs pin` | — | Pin message silently |
| `.axs prefix <new>` | — | Change prefix |
| `.axs alias <name> <cmd>` | — | Create alias |
| `.axs ping` | `.ping` | Latency check |
| `.axs connections` | — | System status |
| `.axs version` | — | Show version |
| `.axs improve` | `.im` | Improve text with AI |

---

## 🏗 Project Structure

```
ApiXS/
├── config.py           # Configuration (env vars → Config dataclass)
├── handler.py          # Telegram event routing & command dispatcher
├── store.py            # Database layer (MySQL via PyMySQL)
├── ad_store.py         # Anti-delete dedicated storage
├── session_crypto.py   # Fernet session encryption/decryption
├── groq_client.py      # Groq async client
├── command_aliases.py  # Shortcut prefix and command name mappings
└── commands/
    ├── ai.py           # AI chat, improve, translate
    ├── anti_delete.py  # Anti-delete monitoring
    ├── downloader.py   # Video downloader (yt-dlp)
    ├── online.py       # Online presence tracking
    ├── yandex_music.py # Yandex Music now playing
    ├── voice.py        # Whisper transcription
    ├── reminders.py    # Time-based reminders
    └── ...             # Other commands
ApiXS_start.py          # Entry point: starts Telegram sessions
```

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🔗 Links

- Live instance: **https://YOUR_DOMAIN**
- Telegram bot: **@YOUR_BOT**
