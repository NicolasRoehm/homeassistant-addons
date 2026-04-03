# Home Assistant Add-on: doorbell

Orchestrateur IA pour sonnette Somfy. Quand un visiteur sonne à la porte, l'add-on
décroche automatiquement et laisse un agent vocal IA converser avec le visiteur.
Le propriétaire reçoit une notification push et peut surveiller ou prendre le relais
en temps réel depuis l'interface web intégrée à Home Assistant.

## Prérequis

- Add-on **SomfyProtect2MQTT** (ou SomfyProtect2MQTT-dev) installé et opérationnel
- Add-on **Mosquitto** installé et configuré
- **HA Companion App** installée sur Android (pour recevoir les notifications)
- Un compte **OpenAI** avec accès à l'API Realtime (`gpt-4o-realtime-preview`)
- Un **token GitHub PAT** avec accès `repo:read` sur `NicolasRoehm/doorbell`

## Installation

1. Ajouter ce dépôt dans **Paramètres → Add-ons → Boutique → Dépôts** :
   `https://github.com/NicolasRoehm/homeassistant-addons`
2. Trouver **doorbell** dans la boutique et cliquer **Installer**.
3. Configurer les options (voir ci-dessous).
4. Démarrer l'add-on.

## Configuration

```yaml
somfy:
  topic_prefix: somfyProtect2mqtt   # Préfixe MQTT de SomfyProtect2MQTT
  site: "Maison"                    # Nom du site Somfy (tel que dans HA)
  device: "V500"                    # Nom du device caméra Somfy
openai:
  api_key: "sk-..."                 # Clé API OpenAI
  model: gpt-4o-realtime-preview    # Modèle OpenAI Realtime
ha_notify_target: "pixel_9"         # Nom de l'appareil dans HA Companion App
                                    # (visible dans Paramètres → Applications mobiles)
github:
  pat: "ghp_..."                    # Token GitHub PAT (accès repo privé doorbell)
  repo: NicolasRoehm/doorbell       # Repo source (ne pas modifier)
  branch: main                      # Branche à utiliser
debug: false
```

> **Note :** Le champ `mqtt` (host, port, username, password) est auto-découvert
> depuis l'add-on Mosquitto. Aucune configuration manuelle nécessaire.
>
> **Note :** Le `SUPERVISOR_TOKEN` et `HA_API_URL` sont injectés automatiquement
> par HA Supervisor — aucun secret à configurer pour les notifications.

## Mise à jour

Pour déployer une nouvelle version du code source :

1. Pousser les changements sur `NicolasRoehm/doorbell@main`
2. Dans HA → **Add-on doorbell** → cliquer **Rebuild**

Le Dockerfile re-télécharge le code source depuis GitHub à chaque rebuild.
Un simple **Restart** réutilise le code déjà installé (démarrage rapide).

## Interface

L'app Angular est accessible via HA Ingress à l'URL :
```
{url-ha}/api/hassio_ingress/doorbell
```

Elle affiche en temps réel :
- Le flux vidéo (frames JPEG de la caméra Somfy)
- Le transcript de la conversation (IA / visiteur)
- L'état de l'appel (en attente / connecté / terminé)
- Les boutons : **Prendre le relais** (micro) et **Raccrocher**

## Phases d'implémentation

| Phase | Fonctionnalité | Statut |
|---|---|---|
| 1 | Auto-answer MQTT (écoute `call/state: ringing` → répond) | 🔜 à venir |
| 2 | Bridge audio OpenAI Realtime API | 🔜 à venir |
| 3 | WS Server + App Angular (vue observateur) | 🔜 à venir |
| 4 | Prise de relais micro | 🔜 à venir |
| 5 | Notifications push via HA API | 🔜 à venir |
