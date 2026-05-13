# 🎰 RaspiCasino

Un casino en ligne tournant sur Raspberry Pi, avec notifications Discord temps réel et feedback physique via GPIO.

![Stack](https://img.shields.io/badge/Raspberry_Pi-OS_Lite-C51A4A?logo=raspberrypi) ![Node-RED](https://img.shields.io/badge/Node--RED-1880-8F0000?logo=nodered) ![Discord](https://img.shields.io/badge/Discord-Webhook-5865F2?logo=discord) ![HTML](https://img.shields.io/badge/Frontend-HTML%2FJS-F7DF1E?logo=javascript)

---

## 📦 Contenu du projet

```
raspicasino/
├── index.html           # Site web du casino (frontend complet)
├── node-red-flow.json   # Flow Node-RED à importer
└── GUIDE_RASPBERRY.md   # Guide d'installation détaillé
```

---

## 🎮 Jeux disponibles

| Jeu | Règles | Gains max |
|---|---|---|
| 🔴 Roulette | Plein, rouge/noir, pair/impair, douzaines | ×36 la mise |
| 🎲 Dés | Bas/haut, total exact, double | ×35 la mise |
| 🎰 Slots | 3 rouleaux pondérés, 8 symboles | ×50 la mise |
| 🃏 Blackjack | Tirer / Rester / Doubler | ×2.5 (blackjack) |

---

## 🏗️ Architecture

```
[Navigateur :3000]
      │
      │  POST /casino/result (JSON)
      ▼
[Node-RED :1880]
      ├──▶ Discord Webhook ──▶ 📢 Embed win/lose dans votre salon
      ├──▶ GPIO Raspberry Pi ──▶ 💡 LED verte (win) / rouge (lose) + buzzer
      ├──▶ Fichier CSV ──▶ 📁 /home/pi/casino/logs/casino_YYYY-MM-DD.csv
      └──▶ Stats horaires ──▶ 📊 Rapport automatique Discord
```

---

## ⚡ Installation rapide

### 1. Dépendances

```bash
# Node.js 18+
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Node-RED + GPIO
sudo npm install -g --unsafe-perm node-red node-red-node-pi-gpio

# Lancement au démarrage
sudo systemctl enable nodered && sudo systemctl start nodered
```

### 2. Déployer le site

```bash
mkdir -p /home/pi/casino/logs
# Copier index.html dans /home/pi/casino/
cd /home/pi/casino && python3 -m http.server 3000 &
```

### 3. Importer le flow Node-RED

1. Ouvrir `http://raspberrypi.local:1880`
2. Menu ☰ → **Import** → coller `node-red-flow.json`
3. **Deploy**

### 4. Configurer Discord

Dans le nœud **"Discord Webhook"**, remplacer l'URL par votre webhook :

```
https://discord.com/api/webhooks/WEBHOOK_ID/WEBHOOK_TOKEN
```

> Créer un webhook : **Paramètres du salon Discord → Intégrations → Webhooks → Nouveau Webhook**

---

## 🔌 Câblage GPIO

```
GPIO 17  (PIN 11)  →  LED Verte  + résistance 330Ω → GND
GPIO 27  (PIN 13)  →  LED Rouge  + résistance 330Ω → GND
GPIO 22  (PIN 15)  →  Buzzer (passif)              → GND
GND      (PIN 6)   →  Masse commune
```

---

## 📡 Comportement temps réel

Chaque partie jouée déclenche :

- **Victoire** → embed Discord vert + LED verte 2s + buzzer
- **Défaite** → embed Discord rouge + LED rouge 2s
- **Chaque heure** → rapport de stats automatique dans Discord

---

## 🌐 Accès réseau local

```bash
hostname -I   # Trouver l'IP du Pi
```

| Service | Adresse |
|---|---|
| Casino | `http://192.168.X.X:3000` |
| Node-RED | `http://192.168.X.X:1880` |

---

## 📁 Format des logs CSV

```csv
timestamp,game,bet,result,amount,balance
2024-01-15T20:30:00Z,Slots,50,🍒🍒🍒,250,1250
2024-01-15T20:31:15Z,Roulette,100,N°17 (black),-100,1150
```

Stats rapides :
```bash
awk -F',' '{sum+=$5; count++} END {print count" parties | P&L: "sum"€"}' \
  /home/pi/casino/logs/casino_$(date +%Y-%m-%d).csv
```

---

## 🛠️ Stack technique

- **Frontend** : HTML5 / CSS3 / JS vanilla — zéro dépendance, Canvas API pour la roue
- **Backend** : Node-RED — orchestration des événements
- **Hardware** : Raspberry Pi GPIO via `node-red-node-pi-gpio`
- **Notifications** : Discord Webhooks (embeds rich)
- **Logs** : CSV plat, un fichier par jour

---

*Pour usage privé uniquement. Le jeu peut créer une dépendance.*
