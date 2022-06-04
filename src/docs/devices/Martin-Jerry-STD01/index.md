---
title: Martin Jerry STD01
date-published: 2022-01-23
type: dimmer
standard: us
---

Martin Jerry STD01 3-Way Switch![image](Treatlife-DS02S.png)

[Amazon Link](https://www.amazon.com/dp/B07N6CJW42)

## Notes

This TuyaMCU requires a baud rate of 115200.

When you disassemble the device, mind the thumbwheel at the bottom of the switch.  You need to pull it apart from the top first or the thumbwheel will break off.

The black and red wires on the harness are ground and 3.3V respectively.
You will need to soldier wires onto GPIO0, TX, RX, and the Tuya MCU's reset pin.
The Tuya MCU's reset pin is number 14, and is active **HIGH** - you need to connect it to VCC.
Note that the reset button on the switch itself kills the power supply.

Finally, the device won't function properly if the harness isn't plugged in - the ESP and the Tuya MCU refuse to talk to each other.  I have not figured out why; I reconnected it, hooked it up to AC, and powered it up and insured it works.

## GPIO Pinout

| Pin   | Function |
| ----- | -------- |
| GPIO1 | Tuya Tx  |
| GPIO3 | Tuya Rx  |

## Basic Configuration

```yaml
substitutions:
  device_name: familyroom_light_1 #change
  friendly_name: Family Room Light #change
  icon: "mdi:light-switch"

esphome:
  name: ${device_name}
  platform: ESP8266
  board: esp01_1m

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

logger:
  baud_rate: 0

api:

ota:

uart:
  rx_pin: GPIO3
  tx_pin: GPIO1
  baud_rate: 115200

tuya:

sensor:
  - platform: wifi_signal
    name: ${friendly_name} WiFi Signal
    update_interval: 60s

  - platform: uptime
    name: ${friendly_name} Uptime

binary_sensor:
  - platform: status
    name: ${friendly_name} Status

light:
  - platform: "tuya"
    name: ${friendly_name}
    dimmer_datapoint: 2
    switch_datapoint: 1
    min_value: 20
    max_value: 255
```
