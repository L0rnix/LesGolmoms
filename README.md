# 🎰 Projet Casino Connecté

## 📌 Présentation

Ce projet est un système de **casino connecté** combinant plusieurs technologies :

* 🎮 Un serveur Minecraft
* 🤖 Un bot Discord
* 🍓 Un Raspberry Pi
* 🔌 Des GPIO / pins physiques
* 🔄 Node-RED pour l’automatisation

L’objectif est de créer une expérience interactive où les événements dans Minecraft, Discord et le matériel physique communiquent ensemble en temps réel.

---

# 🏗️ Architecture du projet

```text
Minecraft Server
       │
       ▼
 Discord Bot ─────► Node-RED ◄───── Raspberry Pi GPIO
       │
       └────────► Events ◄────────┘
```

---

# ⚙️ Technologies utilisées

## 🎮 Minecraft

Le serveur Minecraft sert de plateforme principale pour les joueurs.

Fonctionnalités possibles :

* Gestion des joueurs
* Mini-jeux de casino
* Système d’argent virtuel
* Déclenchement d’événements
* Communication avec Discord

Technologies :

* PaperMC / Spigot
* Plugins Java ou scripts
* Événements et automatisation

---

## 🤖 Bot Discord

Le bot Discord permet de contrôler et surveiller le casino.

Fonctionnalités :

* Commandes de gestion
* Notifications en temps réel
* Logs des événements
* Interaction avec Minecraft
* Contrôle du Raspberry Pi

Exemple de commandes :

```bash
/play
/balance
/jackpot
/restart
/status
```

Technologies :

* Node.js
* discord.js

---

## 🍓 Raspberry Pi

Le Raspberry Pi permet de connecter le projet au monde physique.

Fonctionnalités possibles :

* Contrôle des LEDs
* Boutons physiques
* Relais
* Affichage écran LCD
* Détection d’actions physiques

Exemple :

* Un jackpot Minecraft allume une LED physique
* Un bouton physique lance une animation dans Minecraft

---

## 🔌 GPIO / Pins

Les pins GPIO du Raspberry Pi sont utilisées pour connecter différents composants.

Exemples de composants :

* LEDs
* Boutons
* Buzzers
* Écrans
* Relais

---

## 🔄 Node-RED

Node-RED sert de centre d’automatisation.

Fonctionnalités :

* Gestion des flux
* Communication entre services
* Traitement des événements
* Dashboard de monitoring
* MQTT et automatisation

Exemple de flux :

```text
Minecraft Event
      ↓
Discord Notification
      ↓
Activation GPIO
      ↓
Animation LED
```

---

# 📂 Structure du projet

```text
project/
│
├── minecraft-server/
├── discord-bot/
├── node-red/
├── raspberry/
├── scripts/
├── config/
└── README.md
```

---

# ▶️ Démarrage du système

Le projet est conçu pour être contrôlé principalement depuis **Node-RED**.

## 🔄 Fonctionnement

* Le serveur Minecraft est lancé séparément
* Le bot Discord est lancé via Node-RED
* Les scripts Raspberry Pi sont lancés via Node-RED
* Les automatisations et interactions passent par Node-RED

Architecture de lancement :

```text
Minecraft Server (manuel)
        │
        ▼
     Node-RED
    ├── Lance le bot Discord
    ├── Contrôle le Raspberry Pi
    ├── Gère les GPIO
    └── Automatise les événements
```

---

# 🚀 Installation

## 1. Cloner le projet

```bash
git clone https://github.com/votre-projet/casino-connected.git
cd casino-connected
```

---

## 2. Installer le bot Discord

```bash
cd discord-bot
npm install
```

Créer un fichier `.env` :

```env
DISCORD_TOKEN=your_token
CLIENT_ID=your_client_id
GUILD_ID=your_guild_id
```

---

## 3. Installer Node-RED

```bash
npm install -g --unsafe-perm node-red
```

Lancer Node-RED :

```bash
node-red
```

Accès :

```text
http://localhost:1880
```

---

## 4. Configurer le Raspberry Pi

Mettre à jour le système :

```bash
sudo apt update
sudo apt upgrade
```

Installer Node.js :

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

Installer les dépendances GPIO :

```bash
npm install onoff
```

---

## 5. Lancer le projet

### Serveur Minecraft

Le serveur Minecraft doit être lancé manuellement :

```bash
java -jar server.jar
```

---

### Node-RED

Node-RED sert de contrôleur principal du système.

Lancer Node-RED :

```bash
node-red
```

Depuis Node-RED, il est possible de :

* Démarrer le bot Discord
* Lancer les scripts Raspberry Pi
* Contrôler les GPIO
* Automatiser les événements du casino
* Gérer les interactions entre les services

---

# 🔐 Sécurité

* Ne jamais partager les tokens Discord
* Utiliser des variables d’environnement
* Limiter les permissions du bot
* Sécuriser l’accès SSH du Raspberry Pi

---

# 💡 Idées d’améliorations

* 🎰 Machines à sous physiques
* 📊 Dashboard web
* 💸 Système de monnaie avancé
* 🧠 Intelligence artificielle
* 📱 Application mobile
* 🔔 Notifications push

---

# 👨‍💻 Auteurs

Projet développé avec :

* Node.js
* Discord.js
* Minecraft
* Raspberry Pi
* Node-RED

---

# 📜 Licence

Projet open-source sous licence MIT.
