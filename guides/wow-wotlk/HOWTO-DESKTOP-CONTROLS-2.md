# 🖥️ Desktop Mode Controls — Part 2
## GM Console, Commands, Troubleshooting & Linux Basics

> **⬅️ [Back to Part 1 — Server Management & Account Creation](./HOWTO-DESKTOP-CONTROLS-1.md)**

---

## 🖥️ The GM Console — Full Guide

The GM Console is your direct line to the WoW worldserver.
Think of it like texting the game engine directly. You can
create accounts, run commands, check server status — all
without being logged into the game.

> 💡 **What it is:** The worldserver is actually an interactive
> program running inside Docker. The GM console lets you type
> commands directly into that program in real time.

---

### Opening the GM Console

Make sure your server is running first:
```bash
cd ~/wow-server && docker compose up -d
```

Then attach to the worldserver:
```bash
docker attach acore-docker-ac-worldserver-1
```

You'll see the server's output scrolling and a cursor waiting
for input. You're now in the GM console!

> 💡 **NPCBots server?** Use this command instead:
>
> `docker attach ac-worldserver`

---

### ⚠️ The Most Important Thing — How to EXIT Safely

**NEVER press Ctrl+C inside the GM console.**

Ctrl+C kills the worldserver completely — everyone gets
disconnected and you'll have to restart.

**The correct way to exit:**
1. Press and hold **Ctrl+P**
2. Then immediately press **Ctrl+Q**

This detaches you from the console safely while leaving the
server running perfectly.

> 💡 **Memory trick:** Think of it as "P for Pause, Q for Quit
> the console" — two keystrokes, always in that order.

---

### Creating Accounts in the GM Console

```
account create USERNAME PASSWORD PASSWORD
```
*(Yes, type the password twice — it's a confirmation step)*

Set GM level immediately after:
```
account set gmlevel USERNAME 3 -1
```

Full example — creating an account called "caitlin":
```
account create caitlin mypassword mypassword
account set gmlevel caitlin 3 -1
```

Then exit safely: **Ctrl+P then Ctrl+Q**

---

### Common GM Console Commands

```
account create USERNAME PASSWORD PASSWORD  -- create account
account set gmlevel USERNAME 3 -1         -- set GM level
account list                               -- list all accounts
account delete USERNAME                    -- delete account
reload config                              -- reload settings
server info                                -- server stats
server shutdown 10                         -- shutdown in 10s
```

---

### If the Console Won't Accept Input

Sometimes the console appears stuck or ignores typing.
This usually means it's printing log messages over your input.

**Fix — press Enter once** to get a clean line, then type
your command. The command still registers even if you can't
see it clearly.

If that doesn't work, open a second Konsole window and run
commands via the database instead — see the Account Management
section above.

---

### Container Name Quick Reference

Different server versions use different container names:

| Server | GM Console Command |
|---|---|
| Standard WoW | `docker attach acore-docker-ac-worldserver-1` |
| NPCBots WoW | `docker attach ac-worldserver` |

Not sure what your container is called? Run `docker ps` and
look for the worldserver container name in the list.

---

### Spawn a Bot Near You

```
.npcbot spawn CLASS_ID
```

Class IDs:
```
1  = Warrior      2  = Paladin
3  = Hunter       4  = Rogue
5  = Priest       6  = Death Knight
7  = Shaman       8  = Mage
9  = Warlock      11 = Druid
```

Example — spawn a Priest healer:
```
.npcbot spawn 5
```

### Add a Bot to Your Party

Target the bot in-game, then:
```
.npcbot add
```

### Remove a Bot

Target the bot, then:
```
.npcbot remove
```

### Set Bot Role

```
.npcbot set role tank
.npcbot set role heal
.npcbot set role dps
```

### List Your Bots

```
.npcbot list
```

### Tell All Bots to Follow You

```
.npcbot set follow
```

### Tell All Bots to Stay Put

```
.npcbot set standstill

```

> 💡 **Tip:** Install the **NetherBot** addon for a full UI
> so you never have to type these commands!
> github.com/NetherstormX/NetherBot

---

## 🎮 Useful GM Commands In-Game

These are typed in the WoW chat box while playing:

### Teleport Anywhere
```
.tele stormwind
.tele orgrimmar
.tele dalaran
```

### Level Up Your Character
```
.levelup
.levelup 10
```

### Modify Your Speed
```
.modify speed 3
```
*(1 = normal, 3 = fast, 10 = very fast)*

### Give Yourself Gold
```
.modify money 999999
```

### Spawn Any Item
```
.additem ITEM_ID
```
*(Look up item IDs on wowhead.com)*

### Change Time of Day
```
.modify time 12
```
*(0-23 for hour of day)*

### See All Commands
```
.commands
```

---

## 🔧 Troubleshooting Common Problems

### "I can't connect to the server"

**Check the server is running:**
```bash
docker ps
```

If you don't see the containers — start the server:
```bash
cd ~/wow-server && docker compose up -d
```

**Check your realmlist.wtf:**
Open your WoW folder and find `realmlist.wtf`. It should say:
```
set realmlist 127.0.0.1
```

**Give it time:** First launch takes 5-15 minutes.

---

### "My character data is gone!"

Don't panic — your data is stored in a Docker volume which
survives restarts. Check the server started correctly:

```bash
docker logs acore-docker-ac-worldserver-1 | tail -20
```

---

### "The server won't start"

Check what's wrong:
```bash
cd ~/wow-server && docker compose logs | tail -50
```

Most common fix — remove old containers and restart:
```bash
docker compose down
docker compose up -d
```

---

### "I accidentally pressed Ctrl+C in the GM console"

If you pressed Ctrl+C inside the console it may have stopped
the worldserver. Restart it:

```bash
docker restart acore-docker-ac-worldserver-1
```

**Remember:** Always exit the console with **Ctrl+P then Ctrl+Q**

---

## 📚 A Little Bit of Linux

Since you're here — here are a few Linux basics that will
make everything easier. Each one takes 30 seconds to learn.

### Navigating Folders

```bash
cd ~/wow-server      # go into wow-server folder
cd ~                 # go back to home folder
cd ..                # go up one folder
ls                   # list what's in the current folder
pwd                  # show where you currently are
```

### Reading Files

```bash
cat filename.txt     # print the whole file
tail -20 filename    # print the last 20 lines
```

### Stopping a Running Command

**Ctrl+C** — stops whatever is running in the terminal

> ⚠️ Never press Ctrl+C inside the GM console!
> Use Ctrl+P then Ctrl+Q instead.

### Running a Script

```bash
chmod +x script.sh   # give it permission to run
./script.sh          # run it
```

### The Pipe Symbol |

The `|` symbol means "send the output of this to that":

```bash
docker logs ac-worldserver | grep "error"
```

This gets the logs AND searches them for the word "error".
Very useful for finding problems!

---

## 🌐 Two Server Versions — How to Switch

If you have both the standard server AND the NPCBots server:

**Standard WoW server:**
```bash
cd ~/wow-server && docker compose up -d
```

**NPCBots server:**
```bash
cd ~/wow-server-npcbots && docker compose up -d
```

> ⚠️ **Don't run both at the same time!** They use the same
> ports (3724 and 8085). Start one, play, stop it, then
> start the other.

Stop whichever is running:
```bash
cd ~/wow-server && docker compose down
# OR
cd ~/wow-server-npcbots && docker compose down
```

---

## 💾 Backing Up Your Characters

Before doing anything major — back up your character data:

```bash
docker exec wow-server-ac-database-1 mysqldump -uroot -ppassword --databases acore_characters acore_auth acore_world > ~/wow-backup-20260506.sql
```

This saves everything to a file named with today's date.
Keep this file somewhere safe!

**To restore a backup:**
```bash
docker exec -i wow-server-ac-database-1 mysql -uroot -ppassword < ~/wow-backup-20260506.sql
```

---

## 🎓 What You've Learned

If you've read this guide you now know:

- ✅ What Docker containers are and why we use them
- ✅ How to start and stop your server safely
- ✅ How to create and manage accounts
- ✅ How to use the GM console
- ✅ Basic Linux navigation
- ✅ How to read server logs to diagnose problems
- ✅ How to back up your character data

That's genuinely more Linux knowledge than most people have.
And you learned it by setting up a WoW server — not bad! 😄

---

## 📺 Video Guides

Full video tutorials at:
**[youtube.com/@DadsMmoLab](https://youtube.com/@DadsMmoLab)**

---

## 📦 More Guides & Installers

Everything is free at:
**[github.com/DadsMmoLab/dads-mmo-lab](https://github.com/DadsMmoLab/dads-mmo-lab)**

---

*Part of the Dad's MMO Lab project — offline MMO servers
on Steam Deck, free forever.*
