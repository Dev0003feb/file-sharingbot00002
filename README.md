# 🤖 FileShare Pro Bot

A professional Telegram File Sharing Bot with 2-Stage Force Join, shareable links, admin panel, and owner broadcast system.

---

## ✨ Features

- 🔐 **2-Stage Force Join** — 4 channels (Stage 1) + 2 channels (Stage 2)
- 🔗 **Shareable Links** — For files, videos, photos, audio, text, stickers, GIFs
- 📡 **Manual Owner Broadcast** — Send to all users with live progress
- 👑 **Admin Panel** — Full user/file management
- 📊 **Analytics** — Per-file access counts, user stats
- 🌐 **Bilingual** — English + Hindi
- ⚡ **Flood-safe** — Handles Telegram rate limits automatically

---

## 📋 Prerequisites

- Python 3.10 or higher
- A Telegram account
- MongoDB Atlas free account
- A server / VPS / Railway account (for 24/7 hosting)

---

## 🚀 Step-by-Step Setup

### Step 1 — Create Your Bot

1. Open Telegram and search for **@BotFather**
2. Send `/newbot`
3. Enter a name for your bot (e.g., `My File Share Bot`)
4. Enter a username ending in `bot` (e.g., `myfileshare_bot`)
5. BotFather will send you a **Bot Token** — copy it!

### Step 2 — Get Your User ID (Owner ID)

1. Message **@userinfobot** on Telegram
2. It will reply with your numeric User ID (e.g., `123456789`)
3. This is your `OWNER_ID`

### Step 3 — Get Channel IDs

For each of your 6 channels:
1. Forward any message from the channel to **@userinfobot**
2. It shows the channel ID — it starts with `-100` (e.g., `-1001234567890`)
3. Copy all 6 channel IDs

> **Important:** Add your bot as an **Admin** in all 6 channels!

### Step 4 — Set Up MongoDB Atlas (Free)

1. Go to [mongodb.com/atlas](https://www.mongodb.com/atlas) and create a free account
2. Create a new **free cluster** (M0 Sandbox)
3. Under Security → Database Access → Add a new user with a password
4. Under Security → Network Access → Add IP `0.0.0.0/0` (allow all)
5. Click **Connect** → **Connect your application**
6. Copy the connection string — it looks like:
   ```
   mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/
   ```
7. Replace `<password>` with your actual password

### Step 5 — Install the Bot

```bash
# Clone or download the project
git clone https://github.com/yourrepo/FileShareBot
cd FileShareBot

# Install dependencies
pip install -r requirements.txt

# Copy the example env file
cp .env.example .env
```

### Step 6 — Configure .env

Open `.env` in any text editor and fill in all values:

```env
BOT_TOKEN=1234567890:ABCdefGhIjklMNOpqrSTUvwxYZ
BOT_USERNAME=myfileshare_bot
OWNER_ID=123456789
ADMIN_IDS=111111111,222222222

MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/
DB_NAME=filesharebotdb

CHANNEL_1_ID=-1001234567890
CHANNEL_1_LINK=https://t.me/mychannel1
CHANNEL_1_NAME=My Channel 1

# ... fill all 6 channels
```

### Step 7 — Run the Bot

```bash
python bot.py
```

You should see:
```
INFO | MongoDB connected successfully.
INFO | Bot started! Owner: 123456789
INFO | Starting polling...
```

---

## 🖥 Running 24/7

### Option A — Railway (Recommended, Free)

1. Go to [railway.app](https://railway.app) and sign up
2. New Project → Deploy from GitHub Repo (or upload files)
3. Add all environment variables in Railway's Variables tab
4. Deploy! Railway keeps it running 24/7

### Option B — VPS with screen

```bash
screen -S filesharebot
python bot.py
# Press Ctrl+A then D to detach
# Bot keeps running in background
```

### Option C — systemd (Linux VPS)

```bash
# Create service file
sudo nano /etc/systemd/system/filesharebot.service
```

Paste:
```ini
[Unit]
Description=FileShare Telegram Bot
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/FileShareBot
ExecStart=/usr/bin/python3 bot.py
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl enable filesharebot
sudo systemctl start filesharebot
sudo systemctl status filesharebot
```

---

## 📱 BotFather Setup

After the bot is running, set it up in BotFather:

```
/setcommands
```

Paste these commands:
```
start - Start the bot / Main menu
help - Help and instructions
mylinks - View your uploaded files
mystats - Your personal statistics
settings - Manage your preferences
```

Also run:
- `/setdescription` — describe what your bot does
- `/setabouttext` — short about text
- `/setuserpic` — add a bot profile picture

---

## ✅ Testing Checklist

After setup, test each feature:

- [ ] Open bot as a new user — should see Stage 1 force join
- [ ] Join all 4 Stage 1 channels → click verify → should see Stage 2
- [ ] Join both Stage 2 channels → click verify → should see main menu
- [ ] Send a file → should get shareable link
- [ ] Open the shareable link from a different account
- [ ] Test `/owner` command (only works for OWNER_ID)
- [ ] Test owner broadcast: click "📡 Send to All Users" → send a message → choose Test → verify you receive it
- [ ] Test `/admin` panel → statistics, user list

---

## 🔒 Security Notes

- Never share your `.env` file or Bot Token publicly
- Your `OWNER_ID` is the only one who can use `/owner`, `/broadcast`, `/addadmin`
- Admins listed in `ADMIN_IDS` can use `/admin`, `/ban`, `/stats`
- Regular users cannot access any admin features

---

## 📁 Project Structure

```
FileShareBot/
├── bot.py                    ← Start here
├── config.py                 ← All settings
├── requirements.txt
├── .env                      ← Your private config (never share!)
├── handlers/
│   ├── start.py              ← /start, deep links
│   ├── force_join.py         ← 2-stage force join
│   ├── upload.py             ← File uploads
│   ├── links.py              ← Link management
│   ├── admin.py              ← Admin panel
│   ├── owner.py              ← Owner panel
│   ├── broadcast.py          ← Broadcast engine
│   └── settings.py           ← User settings
├── database/
│   ├── mongodb.py            ← DB connection
│   ├── users.py              ← User operations
│   ├── files.py              ← File operations
│   └── broadcasts.py         ← Broadcast history
└── utils/
    ├── keyboards.py           ← All buttons
    ├── messages.py            ← All message text
    ├── helpers.py             ← Utility functions
    └── decorators.py          ← Auth decorators
```

---

## ❓ Common Issues

| Problem | Solution |
|---|---|
| `Missing env variable` | Check all values are filled in `.env` |
| `MongoDB connection failed` | Check URI, whitelist IP `0.0.0.0/0` in Atlas |
| Bot doesn't check channel membership | Make sure bot is **Admin** in all 6 channels |
| `Forbidden` error on broadcast | User blocked the bot — auto-handled |
| Bot not responding | Check `python bot.py` is still running |

---

## 📞 Support

If you have issues, check the bot logs — all errors are printed with details.

Built with ❤️ using python-telegram-bot v20+
