# 🖥️ Desktop Mode Controls — Complete Guide

> **"Give a man a fish and he eats for a day.**
> **Teach a man to fish and he eats forever."**
>
> This guide teaches you how to actually control your WoW server
> from Desktop Mode. Not just the commands — but WHY they work.
> By the end you'll feel comfortable in the terminal and maybe
> even start to enjoy it. 😄

---

## 📖 Quick Reference — The Commands You'll Use Most

Keep this section bookmarked. These are your everyday commands.

### Starting the Server
```bash
cd ~/wow-server && docker compose up -d
```

### Stopping the Server (safely!)
```bash
cd ~/wow-server && docker compose down
```

### Checking Server Status
```bash
docker ps
```

### Watching the Server Start Up
```bash
docker logs -f acore-docker-ac-worldserver-1
```
*(Press Ctrl+C to stop watching — server keeps running)*

### Opening the GM Console
```bash
docker attach acore-docker-ac-worldserver-1
```
*(Exit with Ctrl+P then Ctrl+Q — do NOT press Ctrl+C!)*

---

## 🧠 Understanding What's Actually Happening

Before we dive in — let's understand what your WoW server
actually is. This will make everything click.

### What is Docker?

Think of Docker like a lunchbox. Inside the lunchbox are
everything your WoW server needs — the database, the game
server, all the settings. Docker keeps it all contained and
neat so it doesn't interfere with the rest of your Steam Deck.

When you run `docker compose up` you're opening the lunchbox.
When you run `docker compose down` you're closing it safely.

### What are Containers?

Your WoW server is actually THREE separate programs running
at the same time:

| Container | What it does |
|---|---|
| `ac-database` | Stores everything — characters, items, quests |
| `ac-authserver` | Handles logging in — checks your username and password |
| `ac-worldserver` | The actual game world — NPCs, quests, combat |

They work together. If the database isn't running, nothing works.
That's why we always use `docker compose` — it starts them all
in the right order automatically.

### What is Konsole?

Konsole is the terminal — your direct line to Linux. It's like
texting the operating system. You type a command, Linux does it.

Don't be intimidated by it. Every command in this guide is
safe to run. The worst that can happen is an error message.

---

## 🚀 Opening Konsole

From Gaming Mode:
1. Press **STEAM button**
2. Select **Power**
3. Select **Switch to Desktop**
4. Right-click the desktop → **Open Terminal** or search for **Konsole**

From Desktop Mode:
1. Right-click the desktop background
2. Select **Open Terminal**

---

## ⚡ Everyday Server Management

### Starting Your Server

```bash
cd ~/wow-server && docker compose up -d
```

**What this means:**
- `cd ~/wow-server` — navigate into your server folder
- `docker compose up` — start all the containers
- `-d` — run in the background (d = detached)

After running this, your server starts in the background.
You can close Konsole and it keeps running!

---

### Stopping Your Server Safely

```bash
cd ~/wow-server && docker compose down
```

**Always use this to stop the server** — never just turn off
your Steam Deck while the server is running. Docker needs to
save the database properly first.

If you forget and it shuts down uncleanly don't panic — the
database is usually fine. But this is the safe way.

---

### Checking If Your Server Is Running

```bash
docker ps
```

You'll see a table of running containers. If you see
`ac-worldserver`, `ac-authserver` and `ac-database` — your
server is running. If the table is empty — it's stopped.

---

### Restarting Just the World Server

Sometimes you change a setting and just need to restart the
game world without touching the database:

```bash
docker restart acore-docker-ac-worldserver-1
```

This is faster than a full stop and start.

---

### Checking Server Logs (What's Happening?)

```bash
docker logs acore-docker-ac-worldserver-1
```

This shows everything the server has printed since it started.
To watch it live as it happens:

```bash
docker logs -f acore-docker-ac-worldserver-1
```

The `-f` means follow — it keeps updating in real time.
Press **Ctrl+C** to stop following. The server keeps running.

---

## 👤 Account Management

### Creating a New Account

1. Open the GM console:
```bash
docker attach acore-docker-ac-worldserver-1
```

2. Type the account create command:
```
account create USERNAME PASSWORD PASSWORD
```

3. Set GM level (3 = full admin):
```
account set gmlevel USERNAME 3 -1
```

4. Exit the console safely:
**Ctrl+P** then **Ctrl+Q**

> 💡 **Why type the password twice?** It's a confirmation
> step — like when websites ask you to confirm your password.
> Both must match or it won't create the account.

---

### Creating Multiple Accounts

Want separate accounts for different characters or for
friends on LAN? Just repeat the process:

```
account create caitlin mypassword mypassword
account set gmlevel caitlin 3 -1

account create kiddo1 simplepass simplepass
account set gmlevel kiddo1 1 -1
```

> 💡 **GM Levels explained:**
> - Level **0** = Regular player (no commands)
> - Level **1** = Moderator (basic commands)
> - Level **2** = Game Master (most commands)
> - Level **3** = Administrator (full control)

---

### Changing a Password

```
account set password USERNAME OLDPASSWORD NEWPASSWORD
```

---

### Listing All Accounts

```
account list
```

---

---

## 📖 Continue Reading

**➡️ [Part 2 — GM Console, Bot Commands, Troubleshooting & Linux Basics](./HOWTO-DESKTOP-CONTROLS-2.md)**

---

*Part of the [Dad's MMO Lab](https://github.com/DadsMmoLab/dads-mmo-lab) project — offline MMO servers on Steam Deck, free forever.*
