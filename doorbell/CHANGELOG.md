# Changelog — doorbell

## 0.1.8 — 2026-04-04

### Changed

- `SOMFY_SITE` accepte maintenant un **nom lisible** (ex. `Maison`) en plus d’un UUID ou d’une valeur vide
  - UUID → filtre direct, aucun changement
  - Nom (ex. `Maison`) → résolutions automatique de l’UUID depuis les topics `/info` retenus au démarrage
  - Vide → premier site vu accepté (comportement inchangé)
- Filtrage site désormais dynamique (client.ts) plutôt que statique à la compilation

## 0.1.7 — 2026-04-04

### Added

- La liste de tous les sites Somfy connus est maintenant visible dans `GET /debug/discovery` avec leur `site_id` et leur label (ex. `"Maison"`)
- Abonnement au topic `{prefix}/+/info` : peuplé automatiquement par SomfyProtect2MQTT à son démarrage

## 0.1.6 — 2026-04-04

### Changed

- `SOMFY_SITE` est maintenant optionnel : si absent (champ vide), l’orchestrateur découvre automatiquement le `site_id` et le `device_id` depuis le premier message MQTT reçu
- Le schéma HA passe `somfy.site` en `str?` (optionnel)
- Endpoint `GET /debug/discovery` : retourne le `site_id` et `device_id` découverts (ou un message d’attente) — accessible via Ingress de l’addon

## 0.1.5 — 2026-04-04

### Fixed

- L'orchestrateur s'abonne maintenant au topic `/ringing` en plus de `call/state` comme fallback de découverte
- Protection contre les double-déclenchements si `ringing` et `call/state` arrivent simultanément
- Log de diagnostic au démarrage : topics MQTT abonnés affichés pour vérifier que `SOMFY_SITE` correspond bien au site_id de SomfyProtect2MQTT

## 0.1.4 — 2026-04-04

### Added

- Ajout des options `openai.voice` et `openai.system_prompt` dans la configuration de l'addon
- La voix OpenAI Realtime est maintenant configurable (défaut : `nova`)
- Le prompt système est maintenant personnalisable sans modifier le code

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
