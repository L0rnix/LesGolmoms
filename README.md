# 🎰 Casino Automation Suite

Package complet de casino web avec automatisation Node-RED, notifications Discord et intégration Raspberry Pi GPIO.

## 📦 Contenu

### `index.html`
Application web casino en HTML/CSS/JavaScript contenant 4 jeux :

- 🔴 **Roulette**
  - Paris plein
  - Rouge / Noir
  - Pair / Impair
  - Douzaines
  - Animation roue Canvas

- 🎲 **Dice**
  - 2 dés animés
  - 8 types de paris
  - Multiplicateurs automatiques

- 🎰 **Slots**
  - 3 rouleaux animés
  - Probabilités pondérées
  - Multiplicateurs :
    - 7️⃣ x50
    - 💎 x20
    - ⭐ x10
    - 🍒 x5

- 🃏 **Blackjack**
  - Deck réel
  - Tirer
  - Rester
  - Doubler
  - Calcul automatique du score

### Communication API
Après chaque partie, envoi automatique d’un résultat JSON vers Node-RED :

```bash
POST http://localhost:1880/casino/result
```

Exemple payload :

```json
{
  "game": "roulette",
  "result": "win",
  "amount": 50,
  "timestamp": 1710000000
}
```

---

## ⚙️ `node-red-flow.json`

Flow importable directement dans Node-RED.

### Fonctionnalités

- Réception API depuis le site
- Gestion CORS
- Routing victoire / défaite
- Notifications Discord webhook
- Contrôle GPIO Raspberry Pi

### Actions automatiques

#### Win
- Embed Discord vert
- LED verte ON 2 sec
- Buzzer activation

#### Lose
- Embed Discord rouge
- LED rouge ON 2 sec

### Logging
Logs CSV journaliers :

```bash
/home/pi/casino/logs/
```

Format :

```csv
date,game,result,amount
```

### Reporting
Rapport automatique toutes les heures sur Discord :

- nombre de parties
- gains
- pertes
- ratio winrate

---

## 🍓 `GUIDE_RASPBERRY.md`

Documentation complète :

- Installation Node.js
- Installation Node-RED
- Activation service systemd
- Configuration GPIO
- Tests curl
- Dépannage

---

## 🔌 Setup rapide

### 1. Installer Node-RED

```bash
bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)
```

### 2. Lancer

```bash
node-red-start
```

Accès :

```bash
http://localhost:1880
```

---

## Discord Webhook

Créer webhook :

1. Discord
2. Paramètres salon
3. Intégrations
4. Webhooks

Puis remplacer URL dans le node :

```txt
Discord Webhook
```

---

## 🚦 GPIO recommandé

| Composant | GPIO |
|---|---|
| LED verte | 17 |
| LED rouge | 27 |
| buzzer | 22 |

---

## Import flow

Dans Node-RED :

```txt
Menu → Import → node-red-flow.json
```

Deploy.

---

## Test API

```bash
curl -X POST http://localhost:1880/casino/result \
-H "Content-Type: application/json" \
-d '{"game":"slots","result":"win","amount":100}'
```

---

## Architecture

```txt
index.html
   ↓ POST JSON
Node-RED
   ├── Discord webhook
   ├── GPIO LEDs
   ├── Buzzer
   └── CSV logs
```

---

## Notes

Projet éducatif / démonstration technique.

Utilisation locale recommandée.

---

## Auteur

LesGolmons
