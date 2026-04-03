# NicolasRoehm — Home Assistant Add-on repository

[![Open your Home Assistant instance and add this repository.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2FNicolasRoehm%2Fhomeassistant-addons)

Dépôt personnel d'add-ons Home Assistant.

## Installation

Dans Home Assistant, aller dans **Paramètres → Add-ons → Boutique des add-ons**, cliquer sur le menu ⋮ en haut à droite, puis **Dépôts**. Ajouter :

```
https://github.com/NicolasRoehm/homeassistant-addons
```

## Add-ons

### [SomfyProtect2MQTT](./SomfyProtect2MQTT)

Pont MQTT pour alarme Somfy Home Alarm (caméra V500 Pro). Canal stable.

Basé sur [NicolasRoehm/SomfyProtect2MQTT-audio](https://github.com/NicolasRoehm/SomfyProtect2MQTT-audio),
fork de [Minims/SomfyProtect2MQTT](https://github.com/Minims/SomfyProtect2MQTT) qui ajoute l'audio
bidirectionnel via MQTT (WebRTC ↔ MQTT, PCM16 16 kHz).

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

### [SomfyProtect2MQTT-dev](./SomfyProtect2MQTT-dev)

Même pont MQTT, canal développement — suit toujours le dernier `master` du fork.
Utile pour tester les nouvelles fonctionnalités avant merge upstream.

> Une fois les fonctionnalités mergées dans [Minims/SomfyProtect2MQTT](https://github.com/Minims/SomfyProtect2MQTT),
> ces add-ons ne seront plus nécessaires.

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

### [doorbell](./doorbell)

Orchestrateur IA pour sonnette Somfy. Quand un visiteur sonne, un agent vocal IA décroche
et répond automatiquement. Le propriétaire reçoit une notification push et peut observer
(vidéo, audio, transcript) ou prendre le relais en temps réel.

> **Repo privé.** Nécessite un token GitHub personnel (PAT) configuré dans les options de l'add-on.

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]


[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
