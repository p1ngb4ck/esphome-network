# `pr/ethernet-runtime`

One part of the topic repo p1ngb4ck/esphome-network. The whole topic, with its description, is the branch `main`.

Change of this branch: Ethernet address and on / off at runtime and saved, the YAML `manual_ip` as an NVS default, no DHCP restart without a link, the EMAC driver only while it has a link and no higher-priority interface is connected

Based on these branches, whose changes it holds and builds on:

- `pr/esp32-nvs-defaults` of p1ngb4ck/esphome-storage-drivers: `add_nvs_default()`: defaults written into an NVS partition image at build time
