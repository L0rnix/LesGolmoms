# 🎰 RaspiCasino — Guide d'installation complet

## Architecture

```
[Navigateur] → [index.html] → POST /casino/result → [Node-RED :1880]
                                                          ├── Discord Webhook (embed)
                                                          ├── GPIO LEDs (Raspberry Pi)
                                                          └── Logs CSV
```

---

## 1. Prérequis matériel (Raspberry Pi)

- Raspberry Pi 3B+ / 4 (ou Zero 2W)
- Carte SD 16 Go minimum (Raspberry Pi OS Lite recommandé)
- 1 LED verte + 1 LED rouge + 1 buzzer passif
- 3 résistances 330Ω
- Câbles jumper

### Schéma de câblage

```
Raspberry Pi GPIO     Composant
─────────────────     ─────────
PIN 11 (GPIO 17)  →   LED Verte (+ résistance 330Ω vers GND)
PIN 13 (GPIO 27)  →   LED Rouge (+ résistance 330Ω vers GND)
PIN 15 (GPIO 22)  →   Buzzer (+ vers PIN, - vers GND)
PIN 6  (GND)      →   Toutes les masses
```

---

## 2. Installation Raspberry Pi OS

```bash
# Après avoir flashé la carte SD avec Raspberry Pi Imager
# Activer SSH dans les options avancées de l'imager

# Connexion SSH
ssh pi@raspberrypi.local

# Mise à jour du système
sudo apt update && sudo apt upgrade -y
```

---

## 3. Installation Node.js & Node-RED

```bash
# Installation Node.js 18+
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Vérification
node --version   # doit afficher v18.x ou supérieur
npm --version

# Installation Node-RED
sudo npm install -g --unsafe-perm node-red

# Installation du nœud GPIO pour Raspberry Pi
npm install -g node-red-node-pi-gpio

# Démarrage Node-RED au boot
sudo systemctl enable nodered.service
sudo systemctl start nodered.service

# Vérifier que Node-RED tourne
sudo systemctl status nodered.service
```

---

## 4. Déploiement du site web

```bash
# Créer le dossier du projet
mkdir -p /home/pi/casino/logs
cd /home/pi/casino

# Copier index.html dans ce dossier (via SCP ou nano)
# Depuis votre machine locale :
scp index.html pi@raspberrypi.local:/home/pi/casino/

# Servir le site avec un serveur statique
sudo npm install -g serve
serve /home/pi/casino -p 3000

# Ou avec Python (déjà installé) :
cd /home/pi/casino && python3 -m http.server 3000 &
```

### Lancement automatique au démarrage

```bash
# Créer un service systemd pour le serveur web
sudo nano /etc/systemd/system/casino-web.service
```

Contenu du fichier :
```ini
[Unit]
Description=RaspiCasino Web Server
After=network.target

[Service]
ExecStart=/usr/bin/python3 -m http.server 3000
WorkingDirectory=/home/pi/casino
User=pi
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable casino-web.service
sudo systemctl start casino-web.service
```

---

## 5. Import du flow Node-RED

1. Ouvrir Node-RED : http://raspberrypi.local:1880
2. Cliquer sur ☰ (menu hamburger) en haut à droite
3. **Import** → **Clipboard**
4. Coller le contenu du fichier `node-red-flow.json`
5. Cliquer **Import**

---

## 6. Configuration Discord

### Créer un webhook Discord

1. Ouvrir Discord → choisir votre serveur
2. Paramètres du canal → **Intégrations** → **Webhooks**
3. **Nouveau Webhook** → donner un nom (ex: "RaspiCasino")
4. Copier l'URL du webhook

### Configurer dans Node-RED

1. Dans le flow importé, double-cliquer sur le nœud **"Discord Webhook"**
2. Remplacer l'URL :
   ```
   https://discord.com/api/webhooks/VOTRE_WEBHOOK_ID/VOTRE_WEBHOOK_TOKEN
   ```
3. Cliquer **Done** puis **Deploy** (bouton rouge en haut à droite)

---

## 7. Test de l'installation

```bash
# Tester le endpoint Node-RED manuellement
curl -X POST http://localhost:1880/casino/result \
  -H "Content-Type: application/json" \
  -d '{"game":"Slots","bet":50,"result":"🍒🍒🍒","amount":150,"balance":1150}'
```

Vous devriez voir :
- ✅ Un message embed dans votre canal Discord
- ✅ La LED verte s'allumer 2 secondes
- ✅ Une ligne ajoutée dans `/home/pi/casino/logs/casino_YYYY-MM-DD.csv`

---

## 8. Accéder au casino depuis le réseau local

```bash
# Trouver l'IP du Raspberry Pi
hostname -I
```

Puis sur n'importe quel appareil du réseau :
- **Site casino** : http://192.168.X.X:3000
- **Node-RED** : http://192.168.X.X:1880

---

## 9. Structure des logs CSV

```
timestamp,game,bet,result,amount,balance
2024-01-15T20:30:00Z,Slots,50,🍒🍒🍒,250,1250
2024-01-15T20:31:15Z,Roulette,100,N°17 (black),-100,1150
2024-01-15T20:32:44Z,Blackjack,75,J:21 vs D:18,75,1225
```

---

## 10. Commandes utiles

```bash
# Voir les logs Node-RED en temps réel
sudo journalctl -f -u nodered

# Redémarrer Node-RED
sudo systemctl restart nodered

# Voir les logs casino du jour
tail -f /home/pi/casino/logs/casino_$(date +%Y-%m-%d).csv

# Statistiques rapides
awk -F',' '{sum+=$5; count++} END {print "Parties:"count, "P&L:"sum"€"}' \
  /home/pi/casino/logs/casino_$(date +%Y-%m-%d).csv
```

---

## 11. Dépannage

| Problème | Solution |
|---|---|
| Node-RED inaccessible | `sudo systemctl restart nodered` |
| CORS bloqué sur le site | Vérifier le nœud "CORS Preflight" dans Node-RED |
| LED ne s'allume pas | Vérifier les PINs GPIO (BCM vs Board) dans les nœuds rpi-gpio |
| Discord ne reçoit rien | Vérifier l'URL du webhook et le déploiement Node-RED |
| Site inaccessible | `sudo systemctl restart casino-web` |

---

*RaspiCasino — Pour usage privé uniquement. Le jeu peut créer une dépendance.*
