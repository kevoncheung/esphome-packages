# ESPHome packages

This repo holds the source of various packages I'm using.

## 硬件
- https://oshwhub.com/jochenzhou/homeassistantai-voice-assistant

## Voice assistant

- ESP32-S3-DevKitC-1-N16R8V
- INMP441
- MAX98357A

Create a new device in the ESPHome dashboard and add the following as its config:

```yaml
esphome:
  name: lc-esp32s3r8n8-va
  friendly_name: LC Voice Assist
  # name_add_mac_suffix: true

substitutions:
  # alexa, hey_jarvis, hey_mycroft, okay_nabu https://ghp.ci/https://raw.githubusercontent.com/esphome/micro-wake-word-models/main/models/okay_nabu.json
  micro_wake_word_model: hey_jarvis

  # https://www.home-assistant.io/voice_control/troubleshooting/#to-tweak-the-assist-audio-configuration-for-your-device
  # voice_assistant_noise_suppression_level: "2"
  # voice_assistant_auto_gain: 31dBFS
  # voice_assistant_volume_multiplier: "2.0"

  # INMP441
  mic_ws_pin: GPIO5
  mic_sck_pin: GPIO6
  mic_sd_pin: GPIO4

  # MAX98357A
  spk_lrc_pin: GPIO11
  spk_bclk_pin: GPIO12
  spk_din_pin: GPIO13

  mute_pin: GPIO9
  mute_pin_threshold: "160000"


esp32:
  flash_size: 8MB

voice_assistant:
   noise_suppression_level: 2
   auto_gain: 31dBFS
   volume_multiplier: 3.0

packages:
  voice-assistant:
    url: https://github.com/JochenZhou/esphome-packages
    ref: main
    files: [esp32-s3-voice-assistant.yaml]
    # Or to use the device as a media player:
    # files: [esp32-s3-voice-assistant.yaml, esp32-s3-media-player.yaml]
    refresh: 0s

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
  ap:
    ssid: "$friendly_name-AP"
    password: "12345678"

i2c:
  sda: GPIO39
  scl: GPIO40
  scan: true
  id: bus_a

sensor:
  - platform: hdc1080
    temperature:
      name: "temperature"
    humidity:
      name: "humidity"
    address: 0x40
    update_interval: 60s
```
