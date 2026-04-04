# Changelog

## 2026.4.9

- fix: restore `forward: True` on `answered_call_from_mobile` — log analysis confirmed the camera has a hard 45-second ring timeout (from the moment the doorbell is pressed) that is only cancelled when it receives `answered_call_from_mobile` relayed through the cloud (`forward: True`); without it the cloud processes the message locally without relaying to the camera, the timer runs to completion, WebRTC is closed by the camera and `device.missed_call` is published; the hypothesis that removing `forward: True` would enable the push notification was incorrect

## 2026.4.8

- fix: restore `forward: True` on `video.webrtc.start` — removing it (v2026.4.7) caused a full regression because the camera never received the start signal and never sent back a `video.webrtc.offer`, leaving the system permanently in ringing state
- fix: send `answered_call_from_mobile` WITHOUT `forward: True` — hypothesis: the Somfy cloud distinguishes proxy-relayed messages (`forward: True`) from direct-client actions; only the latter triggers the "call answered" push notification to the homeowner's phone
- feat: add `[TRACE-WS RECV]` / `[TRACE-WS SEND]` INFO-level log lines for every WebSocket message exchanged with the Somfy cloud (key + site/device/session IDs; full payload for non-sensitive messages) to enable call-flow diagnosis without enabling debug mode

## 2026.4.7

- fix: remove `forward: True` from `video.webrtc.start` — when set, the Somfy cloud just routes the packet directly to the camera without updating its internal call state, so it never sent the "call answered" push notification to the homeowner's phone
- fix: replace one-shot `_awaiting_answer_echo` with persistent `_answered_devices` set — the cloud can send multiple `answered_call_from_mobile` echoes during a session (one for our manual message, one for processing `video.webrtc.start`); the previous guard discarded on first receipt and left all subsequent echoes unblocked, which could terminate the AI session
- fix: clear `_answered_devices` in `_video_webrtc_hang_up`, `_device_missed_call`, and hangup command handler so state is clean for the next ring
- fix: `_device_answered_call_from_monitor` now also clears `_answered_devices` before yielding so a human picking up at the monitor always wins cleanly

## 2026.4.6

- fix: send `answered_call_from_mobile` WebSocket message to the Somfy cloud when answering a call — this is the signal the cloud uses to notify the interior monitor to stop ringing; the previous `prepare_push_to_talk` REST call alone was insufficient
- fix: ignore the cloud echo of our own `answered_call_from_mobile` in `_device_answered_call_from_mobile` — previously the handler unconditionally published `call/state: ended`, which immediately terminated the AI session we had just opened

## 2026.4.5

- fix: on WebRTC/ICE connection failure (`iceConnectionState == failed` or `connectionState == failed`), call `close_session()` instead of `pc.close()` directly — the previous code never published `call/state: ended` to MQTT, leaving the orchestrator permanently stuck in `ringing` state and silently blocking every subsequent ring

## 2026.4.4

- fix: call `prepare_push_to_talk` via the Somfy REST API before starting the WebRTC stream when answering a call — without this, `video.webrtc.start` was forwarded directly to the camera (forward=True) without the Somfy cloud registering the call as answered, causing the interior display to keep ringing and the camera speaker to remain in ring-tone mode instead of talk mode

## 2026.4.3

- fix: split large audio/outbound MQTT payloads into 20 ms sub-chunks in `LiveAudioTrack.inject_pcm` — previously only the first 20 ms of each AI audio packet was played on the camera speaker, the rest was silently discarded
- fix: demote `_handle_audio_outbound` debug log to warning when no LiveAudioTrack is registered (easier diagnosis)
- chore: add first-packet info logs for audio/outbound injection and `recv()` to aid debugging

## 2026.4.2

- feat: publish `{prefix}/{site_id}/info` (retained) with `{"site_id": "...", "label": "Maison"}` at startup for each configured site

## 2026.4.1

- fix: always publish `call/state: ringing` on ring event (was gated on `audio.enabled`, blocking external consumers)
- fix: publish `call/state: ended` when call is answered from monitor or mobile app
- fix: publish `call/state: ended` on missed call event

## 2026.4.0

- feat: added audio section to addon schema (enabled, sample_rate, channels)

## 2026.3.1

- Fixed: Somfy SSO token rate-limit handling (retry-after honored, retry/backoff, improved logs)
- Fixed: MQTT startup broker connection context logging (host/port/ssl) with clearer init failure

## 2026.3.0

- Refactor: Major codebase refactor
- Fixed: Issues [#241](https://github.com/Minims/SomfyProtect2MQTT/issues/241), [#240](https://github.com/Minims/SomfyProtect2MQTT/issues/240), [#239](https://github.com/Minims/SomfyProtect2MQTT/issues/239)

## 2026.1.1

- Fixed: Boolean conversion [#236](https://github.com/Minims/SomfyProtect2MQTT/issues/236)

## 2026.1.0

- Fixed: No file descriptor available
- Fixed: Manually trigger alarm
- Breaking: WebRTC stream changed from MJPEG to HLS with audio
- Added: HLS stream for WebRTC cameras (`http://0.0.0.0:8090/<device_id>/playlist.m3u8`)

## 2025.12.4

- Fixed: WebRTC stream errors

## 2025.12.3

- Fixed: Authentication issue

## 2025.12.2

- Added: WebRTC stream support

## 2025.12.1

- Changed: Use MQTT Home Assistant Addon by default if MQTT config is missing
- Improved: Async updates for Site & Devices actions
- Added: DoorLock, update device on lock/unlock trigger
- Fixed: IndoorCamera night vision

## 2025.12.0

- Added: DoorLock, more sensors

## 2025.7.0

- Fixed: VisioPhone, write video to media folder

## 2025.3.0

- Added: Video backend change option
- Fixed: Somfy API breaking change (LINK Update 2.13.0)

## 2025.2.2

- Fixed: Config directory for go2rtc & echo
- Improved: History, first run publishes old messages

## 2025.2.1c

- Fixed: musl alpine dependency

## 2025.2.1b

- Fixed: Test armv7 libjxl

## 2025.2.1a

- Fixed: Installation/upgrade issue

## 2025.2.0

- Fixed: Restore 2025.1.0 version with x86_64 fix
- Added: Smoke binary sensor

## 2025.1.5

- Rolled back: Use SomfyProtect2MQTT 2024.9.0
- Changed: Moved to SomfyProtect2MQTT-dev

## 2025.1.0

- Added: Videophone (ringing, snapshot, open gate, etc.)
- Added: History
- Added: Video events
- Improved: VisioPhone, write clips and snapshots to media folder
- Fixed: Use asyncio for websocket

## 2024.9.0

- Fixed: Resubscribe to all topics on MQTT reconnect

## 2024.3.0

- Fixed: Remove human_detect_enabled
- Changed: Set version to 2024.3.0
- Added: Device class motion for PIR, safety for IntelliTag

## 2024.1.0

- Fixed: Default value for update_available

## 2023.12.2

- Fixed: Immediate alarm status on update

## 2023.12.1

- Fixed: Reduce WebSocket restart time

## 2023.12.0

- Improved: WebSocket
- Updated: Requirements

## 2023.11.0

- Fixed: Do not request snapshot if shutter is closed

## 2023.10.0

- Added: Camera streaming via MQTT Camera or WebRTC Camera (go2rtc)

## 2023.9.4

- Fixed: CPU load

## 2023.9.3

- Fixed: MQTT True/False command
- Fixed: Restart process on failure

## 2023.9.2

- Fixed: supported_features to 14 ([#55](https://github.com/Minims/SomfyProtect2MQTT/issues/55))
  - Manual intervention required: Remove alarm device, restart addon, update Lovelace entity & automation

## 2023.9.1

- Fixed: Siren buttons
- Added: Beta video streaming ([Video Streaming](https://github.com/Minims/SomfyProtect2MQTT#video-streaming))

## 2023.8.3

- Added: Watermark on snapshot

## 2023.8.2

- Added: Siren buttons
- Updated: MQTT for HA 2023.8.0
- Improved: WebSocket

## 2023.8.1

- Fixed: Load issue
- Changed: Retain all MQTT messages

## 2023.8.0

- Improved: WebSocket messages

## 2023.7.0

- Updated: Requirements
- Fixed: Installation failure (PyYaml)

## 2023.6.1

- Cleaned: Old binary_sensor device_lost

## 2023.6.0

- Changed: Versioning to 2023.6.0
- Added: Detailed logs for debugging
- Updated: Requirements
- Added: Scenarios in logs for future features
- Improved: WebSocket
- Breaking: device_lost binary_sensor updated to device_tracker
- Fixed: Typo in device_lost/presence

## 0.2.9

- Fixed: ambient_light_threshold & smart_alarm_duration

## 0.2.8

- Fixed: smart_alarm_duration & lighting_trigger

## 0.2.7

- Changed: Wait 2 sec to update device after action
- Added: GET scenarios-core & scenario (future use)
- Updated: README
- Added: Reboot/Halt button for Camera and Link
- Fixed: smart_alarm_duration

## 0.2.6

- Updated: Device after action
- Updated: shutter_state switch
- Fixed: lighting_state switch
- Updated: lighting_state
- Added: New sensors/switches
- Added: Generic update device
- Added: User API calls and model
- Updated: Action list
- Changed: Version to 0.2.6
- Removed: signal_strength device_class on % signal

## 0.2.5

- Added: Debug log on Site and Devices
- Added: interndoor type

## 0.2.4

- Fixed: security.level.change

## 0.2.3

- Fixed: battery_low

## 0.2.2

- Fixed: recalibration_required

## 0.2.1

- Added: IntelliTag motion sensor (alpha)
- Fixed: Update device/site

## 0.2.0

- Fixed: IntelliTag door type
- Fixed: Camera video type
- Added: Somfy One/One+
- Improved: Logging

## 0.1.9.8

- Updated: Add-on to new S6 ([S6 Overlay Base Images](https://developers.home-assistant.io/blog/2022/05/12/s6-overlay-base-images/))

## 0.1.9

- Added: New entities
- Fixed: WebSocket refresh token
- Added: Motion sensor when alarm is triggered

## 0.1.8

- Added: MQTT SSL
- Updated: SomfyProtect2MQTT

## 0.1.7

- Fixed: request_token

## 0.1.6

- Added: Manual snapshot mode
- Added: Old MyFox security camera
- Added: Smoke detector
- Added: Extender
- Changed: Default code value to 0

## 0.1.5

- Fixed: homeassistant_config schema

## 0.1.4

- Updated: SomfyProtect2MQTT 0.1.3
- Fixed: Outdoor camera snapshot
- Added: Setup code on alarm panel
- Allowed: Disable code on arm/disarm

## 0.1.3

- Updated: SomfyProtect2MQTT 0.1.2
- Fixed: Stop alarm
- Fixed: Trigger alarm
- Do not publish unwanted devices

## 0.1.2

- Added: build.json file (thanks @Minims)

## 0.1.1

- Changed: tar link

## 0.1.0

- Renamed: Addon to "SomfyProtect2MQTT-dev"
- Rewritten: Major code changes (thanks @Minims)
- Based on dev branch
- Added: Logo, icon, changelog, docs

## 0.0.1 to 0.0.4

- Initial version with minor fixes
