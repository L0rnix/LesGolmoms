# 🚀 Serveur Minecraft Custom / Fun Creative Pack

Un serveur Minecraft orienté **fun + créatif + troll + automation**, avec plein de features inutiles mais indispensables.

## Features

### 🍆 Zizi volants
Parce que pourquoi pas.

- Entités custom qui spawn et volent aléatoirement
- Trails particules
- Sons random débiles
- Commandes admin pour spawn/despawn

Commandes :
```bash
/zizi spawn
/zizi nuke
/zizi rain
```

---

## 🎛 Dashboard Web

Panel web interactif pour contrôler le serveur comme un gros bourrin.

### Fonctions
- Start / Stop / Restart serveur
- Kick / Ban / Unban joueurs
- Give items
- Spawn events
- Toggle pluie / jour / nuit
- Boutons troll

### UI
- Boutons énormes
- Stats live
- Console web
- Dark mode obligatoire

Tech stack :
- Node.js
- Express
- Socket.io
- Tailwind / React
- API Minecraft RCON

Exemple dashboard :
- `RESTART SERVER`
- `SPAWN ZIZI`
- `NUKE MAP`
- `DROP TNT`

---

## 📟 LCD Stats Display

Un écran LCD ou panneau affichage live avec stats serveur.

Affiche :
- Joueurs online
- TPS
- RAM usage
- CPU
- Uptime
- Death count
- Kills
- Blocks cassés

Exemple :
```txt
ONLINE: 12/100
TPS: 19.8
RAM: 6.2GB
UPTIME: 3D 12H
```

Compatible :
- LCD I2C
- OLED
- Web overlay

---

## 📜 Logs Discord

Logs automatiques sur Discord.

### Events loggés
- Join / Leave
- Ban / Kick
- Death
- Commandes admin
- Crash serveur
- Backup completed
- Spawn zizi event

Exemple :
```txt
[JOIN] Player123 joined
[CMD] Admin used /zizi rain
[DEATH] Kevin exploded
```

Webhook Discord requis.

---

## ⚡ Commandes custom

### Admin
```bash
/restart
/freeze <player>
/troll <player>
/nuke
/giveall diamond 64
```

### Fun
```bash
/zizi
/rainitems
/randomtp
/yeet
```

### Stats
```bash
/stats
/tps
/ping
/topkills
```

---

## 🤖 Automation

- Auto backup toutes les 30 min
- Auto restart 4h matin
- Crash recovery
- Alert Discord si TPS < 15

---

## Folder Structure

```bash
server/
├── plugins/
├── dashboard/
├── discord-bot/
├── lcd/
├── configs/
└── backups/
```

---

## TODO

- [ ] Mini-jeux débiles
- [ ] Casino illégal
- [ ] IA NPC toxique
- [ ] Boss final zizi géant

---

## Installation

```bash
git clone repo
npm install
npm run start
```

Minecraft :
```bash
java -Xmx8G -jar server.jar nogui
```

---

## Authors

Développé avec beaucoup trop de mauvaises idées.

---

## License

Do whatever you want.
