# homeassistant-addons — Copilot Instructions

## Repository role

This repository contains Home Assistant add-ons published as a custom HA repository.
Each subdirectory (`SomfyProtect2MQTT`, `SomfyProtect2MQTT-dev`, `doorbell`) is an independent add-on.

---

## CRITICAL: Version bump required on every change

**Any modification to an add-on MUST be accompanied by a version bump in that add-on's `config.yaml`.**

Home Assistant uses the `version` field to detect updates and prompt users to upgrade.
Without a bump, users will never receive the change.

### Version format

Versions follow the `YYYY.M.patch` scheme (e.g. `2026.4.0`, `2026.4.1`).

- Increment the **patch** number for bug fixes and minor changes.
- Increment the **month** (and reset patch to `0`) for feature releases.
- Increment the **year** (and reset month + patch) for new yearly cycle.

### Files to update when bumping

For each modified add-on (e.g. `SomfyProtect2MQTT/`):

1. `config.yaml` — update the `version:` field.
2. `CHANGELOG.md` — add a new entry at the top describing the change.

### Example

```yaml
# config.yaml
version: 2026.4.0   # was 2026.3.1
```

```markdown
# CHANGELOG.md

## 2026.4.0

- feat: added audio section to addon schema
```

---

## Add-on schema rules

- Any new config key exposed to the user **must** be declared in both `options` (default value)
  and `schema` (type) inside the add-on's `config.yaml`.
- Home Assistant silently removes keys at save time if they are not in the schema.
- Use `str?`, `int?`, `bool?` for optional fields; omit `?` for required fields.

---

## Do not cross-modify add-ons

Each add-on directory is independent. A change in `SomfyProtect2MQTT/` does not automatically
apply to `SomfyProtect2MQTT-dev/` or `doorbell/`. Update each one explicitly if needed.
