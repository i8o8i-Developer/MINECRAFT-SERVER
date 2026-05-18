# Durgaai Solutions Minecraft Server

> Purpur 1.21.10 | 8 Players | Full Plugin Suite | Deployed Via Coolify And GitHub

## Stack Overview

| Service | Port | Purpose |
| --- | --- | --- |
| Minecraft (Purpur) | `25565` | Java Edition Gameplay |
| Simple Voice Chat | `24454/udp` | In-Game Proximity Voice |
| Dynmap Live Map | `8123` | Browser World Map |
| File Browser | `8080` | Password-Protected Server File Manager |
| Rcon Web Console | `4326` | Password-Protected Browser Rcon Terminal |
| Auto Backup | - | 24-Hour Backups With 7-Day Retention |

## Plugin Suite

| Plugin | Purpose |
| --- | --- |
| LuckPerms | Permissions System |
| Vault | Economy And Permissions API Bridge |
| EssentialsX Suite | Core Commands, Chat Formatting, Spawn Control |
| SkinsRestorer | Player Skins In Online And Offline Mode |
| Simple Voice Chat | In-Game Proximity Voice |
| Dynmap | Live Browser Map |
| ViaVersion And ViaBackwards | Cross-Version Client Compatibility |
| WorldGuard And WorldEdit | Region Protection And Build Utilities |
| CoreProtect CE | Grief Logging And Rollback |
| PlaceholderAPI | Placeholder Integration |
| Spark | Performance Profiling |
| DeathChest | Item Protection On Death |
| PvPManager | PvP State Management |

## Deploy To Coolify From GitHub

### Step 1: Push Repository

```bash
git init
git add .
git commit -m "Initial Durgaai Solutions Minecraft Server"
git remote add origin https://github.com/YourUsername/DurgaaiMinecraftServer.git
git push -u origin main
```

### Step 2: Connect Source In Coolify

Connect Your GitHub Account In `Settings -> Source`.

### Step 3: Create Resource

1. Open `Projects -> New Resource`
2. Choose `Git Repository`
3. Set Build Pack To `Docker Compose`
4. Set Base Directory To `/`
5. Set Compose File To `docker-compose.yml`

### Step 4: Set Environment Variables

```env
SERVICE_PASSWORD_RCON=YourStrongRconPassword
RCON_WEB_USER=Admin
RCON_WEB_PASS=YourStrongRconWebPassword
MINECRAFT_VERSION=1.21.10
MINECRAFT_MAX_MEMORY=8G
MINECRAFT_INIT_MEMORY=8G
PORT=25565
```

### Step 5: Open Firewall Ports

```bash
ufw allow 25565/tcp
ufw allow 24454/udp
ufw allow 8123/tcp
ufw allow 8080/tcp
ufw allow 4326/tcp
ufw allow 4327/tcp
```

### Step 6: Deploy

Deploy From Coolify And Monitor Logs Until Server Health Is Green.

## Post-Deploy Setup

### Grant Operator Access

```text
op YourMinecraftUsername
```

### Configure File Browser Credentials

Configured In Compose As:
- Username: `i8o8i`
- Password: `i8o8i`

## Repository Structure

```text
MINECRAFT-SERVER/
|- docker-compose.yml
|- env.example
|- ReadMe.md
|- HOSTING_GUIDE.md
|- NginxDynmap.conf
|- Dashboard/
|  |- index.html
|- FileBrowserData/
|- FileBrowserConfig/
```
