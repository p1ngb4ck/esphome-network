# esphome-network

## Purpose
Networking for ESPHome: several interfaces (WiFi, Ethernet, USB Ethernet) in a priority order changeable at runtime,
WiFi networks and Ethernet settings at runtime, the device name from the NVS, Improv serial setup, the way each entity
reaches Home Assistant (API, MQTT), `esp32_hosted` update sources and `boot_announce`.

## How it works
- `network` keeps the interfaces in a `priority` order; the first connected one carries the default route and DNS.
  The order is changeable at runtime and saved.
- `wifi` (saved networks, browse scan) and `ethernet` (address, on / off) keep runtime settings saved; the
  EMAC driver runs only while it has a link and no higher-priority interface is connected.
- `name_from_nvs` gives every network component the device name from the NVS (one firmware for many devices);
  `improv_serial` sets interfaces and identity over serial.
- With `api:` and `mqtt:` each entity reaches Home Assistant one way; `esp32_hosted` updates the coprocessor from
  embedded, HTTP or storage sources; `boot_announce` sends a UDP packet after an armed restart.

## Quick start
Chips: ESP32, ESP32-S2, ESP32-S3
Needs: [esphome-esp32-platform](https://github.com/p1ngb4ck/esphome-esp32-platform)
```yaml
external_components:
  - source: github://p1ngb4ck/esphome-network@main
    components: [network, wifi, mdns, api]
  - source: github://p1ngb4ck/esphome-esp32-platform@main
    components: [esp32]
wifi:
  saved_networks: 8
api:
```
Ethernet first: add `ethernet` and `network: priority: [ethernet, wifi]`. ESP32-P4 WiFi needs `esp32_hosted` too.

## Main options

| Option | What it does | Default |
|---|---|---|
| [`network: priority`](CONFIGURATION.md#network) | interfaces in order; the first connected one is the default | - |
| [`on_active_interface`](CONFIGURATION.md#network) | the active interface changed | - |
| [`name_from_nvs`](CONFIGURATION.md#network) | name and friendly name from the NVS `namespace` | - |
| [`wifi: saved_networks`](CONFIGURATION.md#wifi) | room for 1-16 networks added at runtime | - |
| [`on_scan_results`](CONFIGURATION.md#wifi) | a scan finished; results for UIs | - |
| [`improv_serial: identity`](CONFIGURATION.md#improv_serial) | Improv RPCs set name, friendly name, API key | - |
| [`mqtt: publish_default`](CONFIGURATION.md#api-or-mqtt-per-entity) | `api`, `mqtt` or `both` per entity | `both` |
| [`esp32_hosted` `type`](CONFIGURATION.md#esp32_hosted-update-sources) | `embedded`, `http`, `storage` | - |
| [`boot_announce: udp_id`](CONFIGURATION.md#boot_announce) | the `udp` component for the boot packet | - |

## Catalog
- Components (folders on main; options):
  [network](https://github.com/p1ngb4ck/esphome-network/tree/main/esphome/components/network)
  ([options](CONFIGURATION.md#network)),
  [wifi](https://github.com/p1ngb4ck/esphome-network/tree/main/esphome/components/wifi)
  ([options](CONFIGURATION.md#wifi)),
  [ethernet](https://github.com/p1ngb4ck/esphome-network/tree/main/esphome/components/ethernet)
  ([options](CONFIGURATION.md#ethernet)),
  [mdns](https://github.com/p1ngb4ck/esphome-network/tree/main/esphome/components/mdns),
  [captive_portal](https://github.com/p1ngb4ck/esphome-network/tree/main/esphome/components/captive_portal),
  [api](https://github.com/p1ngb4ck/esphome-network/tree/main/esphome/components/api),
  [mqtt](https://github.com/p1ngb4ck/esphome-network/tree/main/esphome/components/mqtt)
  ([options](CONFIGURATION.md#api-or-mqtt-per-entity)),
  [improv_serial](https://github.com/p1ngb4ck/esphome-network/tree/main/esphome/components/improv_serial)
  ([options](CONFIGURATION.md#improv_serial)),
  [esp32_hosted](https://github.com/p1ngb4ck/esphome-network/tree/main/esphome/components/esp32_hosted)
  ([options](CONFIGURATION.md#esp32_hosted-update-sources)),
  [boot_announce](https://github.com/p1ngb4ck/esphome-network/tree/main/esphome/components/boot_announce)
  ([options](CONFIGURATION.md#boot_announce)),
  [openthread](https://github.com/p1ngb4ck/esphome-network/tree/main/esphome/components/openthread).
- Branches: [main](https://github.com/p1ngb4ck/esphome-network/tree/main),
  [pr/boot-announce](https://github.com/p1ngb4ck/esphome-network/tree/pr/boot-announce),
  [pr/esp32-hosted-update-sources](https://github.com/p1ngb4ck/esphome-network/tree/pr/esp32-hosted-update-sources),
  [pr/ethernet-runtime](https://github.com/p1ngb4ck/esphome-network/tree/pr/ethernet-runtime),
  [pr/improv-serial-identity](https://github.com/p1ngb4ck/esphome-network/tree/pr/improv-serial-identity),
  [pr/improv-serial-network](https://github.com/p1ngb4ck/esphome-network/tree/pr/improv-serial-network),
  [pr/mqtt-api-publish-way](https://github.com/p1ngb4ck/esphome-network/tree/pr/mqtt-api-publish-way),
  [pr/mqtt-events-free](https://github.com/p1ngb4ck/esphome-network/tree/pr/mqtt-events-free),
  [pr/mqtt-tls-sha512](https://github.com/p1ngb4ck/esphome-network/tree/pr/mqtt-tls-sha512),
  [pr/network-name-from-nvs](https://github.com/p1ngb4ck/esphome-network/tree/pr/network-name-from-nvs),
  [pr/network-nvs-ready](https://github.com/p1ngb4ck/esphome-network/tree/pr/network-nvs-ready),
  [pr/network-priority](https://github.com/p1ngb4ck/esphome-network/tree/pr/network-priority),
  [pr/network-restore-default-dns](https://github.com/p1ngb4ck/esphome-network/tree/pr/network-restore-default-dns),
  [pr/network-tasks-loop-core](https://github.com/p1ngb4ck/esphome-network/tree/pr/network-tasks-loop-core),
  [pr/network-usb-ethernet](https://github.com/p1ngb4ck/esphome-network/tree/pr/network-usb-ethernet),
  [pr/network-wifi-idf61](https://github.com/p1ngb4ck/esphome-network/tree/pr/network-wifi-idf61),
  [pr/wifi-ap-state-callback](https://github.com/p1ngb4ck/esphome-network/tree/pr/wifi-ap-state-callback),
  [pr/wifi-saved-networks](https://github.com/p1ngb4ck/esphome-network/tree/pr/wifi-saved-networks),
  [pr/wifi-startup-done-callback](https://github.com/p1ngb4ck/esphome-network/tree/pr/wifi-startup-done-callback).
- Needed repos: [esphome-esp32-platform](https://github.com/p1ngb4ck/esphome-esp32-platform) (`esp32`); per branch
  [esphome-storage-drivers](https://github.com/p1ngb4ck/esphome-storage-drivers) (`pr/esp32-nvs-ready`,
  `pr/esp32-nvs-defaults`), [esphome-storage-interface](https://github.com/p1ngb4ck/esphome-storage-interface)
  (`storage`, `esp32_hosted` `type: storage`), [esphome-usb-drivers](https://github.com/p1ngb4ck/esphome-usb-drivers)
  (`usb_ethernet`).
- All topics together: `github://p1ngb4ck/esphome@dev`.

---

# Detailed version

Networking changes for ESPHome. Several interfaces (WiFi, Ethernet, USB Ethernet) run with a priority order that can
be changed at runtime and decides the default route. The WiFi network list is changeable at runtime, with scan
results for a UI; Ethernet settings are changeable at runtime. The device name comes from the NVS (one firmware for
many devices). Improv serial sets up interfaces and identity. Each entity reaches Home Assistant one way (API, MQTT
or both). `esp32_hosted` updates the ESP32-C6 firmware from several sources, and `boot_announce` sends a packet to a
waiting host when the device is back after a restart.

## Examples

WiFi and Ethernet, Ethernet preferred while it has a link. The PHY and pins below are those of an ESP32-P4 board
(IP101 over the P4's EMAC); on the ESP32-P4 WiFi also needs `esp32_hosted`.

```yaml
external_components:
  - source: github://p1ngb4ck/esphome-network@main
    components: [network, wifi, ethernet, mdns, api]
  - source: github://p1ngb4ck/esphome-esp32-platform@main
    components: [esp32]

network:
  priority:
    - ethernet
    - wifi

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
  saved_networks: 8

ethernet:
  type: IP101
  mdc_pin: GPIO31
  mdio_pin: GPIO52
  clk:
    pin: GPIO50
    mode: CLK_EXT_IN
  phy_addr: 1
  power_pin: GPIO51
```

Use your board's PHY and pins.

Chips: `network`, `wifi`, `mdns`, `api`, `mqtt`, `improv_serial`, `boot_announce` all chips; Ethernet over EMAC
ESP32, ESP32-P4, ESP32-S31; `esp32_hosted` the hosts (ESP32-P4, ESP32-H2).

Needs: `esp32` from [esphome-esp32-platform](https://github.com/p1ngb4ck/esphome-esp32-platform) (`nvs_ready`,
`LOOP_TASK_CORE`); `esp32_hosted` `type: storage` needs the storage interface
([esphome-storage-interface](https://github.com/p1ngb4ck/esphome-storage-interface)). The other components that carry
this topic's changes - `mqtt`, `improv_serial`, `captive_portal`, `esp32_hosted`, `boot_announce`, `openthread` - are
taken from here as well when a config uses them; `mqtt` from here needs `api` from here.

## More

**[CONFIGURATION.md](CONFIGURATION.md)**: every new option - interface priority and its trigger, saved networks and
scan results, the name from the NVS, Improv identity, API / MQTT per entity, `esp32_hosted` update sources,
`boot_announce`.

## Branches

One branch per change; `main` contains every branch. Base: ESPHome `dev` at `a5a75738ad`.

- `pr/network-priority` (ESP32; needs -): `network` `priority` changeable at runtime and saved,
  `on_active_interface`, the default route on the first connected interface in that order.
- `pr/network-restore-default-dns` (ESP32; needs `pr/network-priority`): the default interface's DNS servers written
  back when another interface's address change cleared them.
- `pr/network-usb-ethernet` (ESP32 with USB host; needs `pr/network-restore-default-dns`): usb_ethernet as a third
  arbitrated interface (network, mdns).
- `pr/network-name-from-nvs` (ESP32; needs `pr/network-priority`): `name_from_nvs`: device name and friendly name from
  the NVS for every network component.
- `pr/network-wifi-idf61` (ESP32; needs -): ESP-IDF 6.1: options removed there skipped (network, wifi).
- `pr/network-nvs-ready` (ESP32; needs `pr/esp32-nvs-ready`): wifi and openthread wait for `nvs_ready`.
- `pr/wifi-saved-networks` (ESP32; needs -): `saved_networks`: a network list changeable at runtime, browse scan,
  `on_scan_results`.
- `pr/wifi-startup-done-callback` (ESP32; needs -): `add_on_startup_done_callback` / `is_startup_done`: once per boot.
- `pr/wifi-ap-state-callback` (ESP32; needs `pr/wifi-startup-done-callback`): `add_on_ap_state_callback`: the access
  point up / down.
- `pr/ethernet-runtime` (ESP32, P4, S31 (EMAC); needs `pr/esp32-nvs-defaults`): Ethernet address and on / off at
  runtime and saved, the YAML `manual_ip` as an NVS default, no DHCP restart without a link, the EMAC driver only
  while it has a link and no higher-priority interface is connected.
- `pr/network-tasks-loop-core` (ESP32; needs `pr/esp32-loop-task-core`): mqtt and ethernet tasks pinned to
  `LOOP_TASK_CORE`.
- `pr/improv-serial-network` (as improv_serial; needs -): Improv serial RPC 0x80: interfaces in priority order, on /
  off, DHCP or a manual address.
- `pr/improv-serial-identity` (ESP32; needs `pr/improv-serial-network`, `pr/network-name-from-nvs`): Improv serial
  RPCs 0x85 / 0x86 / 0x87: name, friendly name, an API key stored.
- `pr/mqtt-api-publish-way` (as mqtt / api; needs -): which way each entity reaches Home Assistant:
  `publish_default`, `publish_api` / `publish_mqtt` / `publish_both`, changeable at runtime.
- `pr/mqtt-events-free` (ESP32; needs -): inbound MQTT events give their topic / payload memory back when released.
- `pr/mqtt-tls-sha512` (ESP32; needs `pr/esp32-cert-bundle-sha512`): MQTT over TLS keeps SHA-384 / SHA-512.
- `pr/esp32-hosted-update-sources` (P4, H2 (hosts); needs `pr/storage`): `esp32_hosted` update: `embedded`, `http`
  and `storage` sources per entity, one non-blocking install sequence, `sha256`.
- `pr/boot-announce` (ESP32; needs `pr/network-priority`): `boot_announce`: after an armed restart, a UDP packet once
  the network is up, repeated until acknowledged.

`pr/esp32-loop-task-core` and `pr/esp32-cert-bundle-sha512` are in
[esphome-esp32-platform](https://github.com/p1ngb4ck/esphome-esp32-platform); `pr/esp32-nvs-ready` and
`pr/esp32-nvs-defaults` are in [esphome-storage-drivers](https://github.com/p1ngb4ck/esphome-storage-drivers);
`pr/storage` in [esphome-storage-interface](https://github.com/p1ngb4ck/esphome-storage-interface); `usb_ethernet`
itself is in [esphome-usb-drivers](https://github.com/p1ngb4ck/esphome-usb-drivers).

All topics together: `github://p1ngb4ck/esphome@dev`. `external_components` replaces components only; the ESPHome
core of the build host stays as installed.

## License

- ESPHome's own files, also where this repository changes them: the [ESPHome License](LICENSE) (MIT for the Python
  code and everything else, GPLv3 for the C++ runtime files).
- New files written for this repository (new components, their code, tests and documentation): MIT,
  [LICENSE-MIT](LICENSE-MIT).

## Thanks

Everyone this project builds on: [ATTRIBUTIONS.md](https://github.com/p1ngb4ck/esphome/blob/dev/ATTRIBUTIONS.md).

## Branch structure

Each `pr/` branch holds one part of this topic on top of upstream and says in its `BRANCH.md` what it is based on.
`main` holds all of them.

| Branch | Based on |
|---|---|
| `pr/boot-announce` | `pr/network-priority` |
| `pr/esp32-hosted-update-sources` | `pr/storage` of p1ngb4ck/esphome-storage-interface |
| `pr/ethernet-runtime` | `pr/esp32-nvs-defaults` of p1ngb4ck/esphome-storage-drivers |
| `pr/improv-serial-identity` | `pr/improv-serial-network`, `pr/network-name-from-nvs` |
| `pr/improv-serial-network` | upstream only |
| `pr/mqtt-api-publish-way` | upstream only |
| `pr/mqtt-events-free` | upstream only |
| `pr/mqtt-tls-sha512` | `pr/esp32-cert-bundle-sha512` of p1ngb4ck/esphome-esp32-platform |
| `pr/network-name-from-nvs` | `pr/network-priority` |
| `pr/network-nvs-ready` | `pr/esp32-nvs-ready` of p1ngb4ck/esphome-storage-drivers |
| `pr/network-priority` | upstream only |
| `pr/network-restore-default-dns` | `pr/network-priority` |
| `pr/network-tasks-loop-core` | `pr/esp32-loop-task-core` of p1ngb4ck/esphome-esp32-platform |
| `pr/network-usb-ethernet` | `pr/network-restore-default-dns` |
| `pr/network-wifi-idf61` | upstream only |
| `pr/wifi-ap-state-callback` | `pr/wifi-startup-done-callback` |
| `pr/wifi-saved-networks` | upstream only |
| `pr/wifi-startup-done-callback` | upstream only |
