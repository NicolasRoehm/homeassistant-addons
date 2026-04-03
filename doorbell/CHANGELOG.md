# Changelog — doorbell

## 0.1.0 — 2026-04-03

### Added

- Structure initiale de l'add-on Home Assistant
- Dockerfile basé sur `ghcr.io/hassio-addons/base:20.0.1` (Alpine 3.20 + Node.js 20 LTS)
- Téléchargement du code source depuis le repo privé `NicolasRoehm/doorbell` via PAT GitHub
- Logique de premier démarrage : download + `npm ci` + `npm run build` au premier lancement, puis démarrage direct sur les redémarrages suivants
- Build Angular automatique si le dossier `app/` est présent (Phase 3+)
- Configuration HA Ingress sur le port 3000
- Injection des variables d'environnement depuis les options de l'add-on (Somfy, OpenAI, Firebase, MQTT)
- Auto-découverte MQTT depuis l'add-on Mosquitto de HA
- Supervision s6-overlay avec restart automatique
