# xiao-ir-mate

ESPHome firmware for the [Seeed XIAO Smart IR Mate](https://www.seeedstudio.com/XIAO-Smart-IR-Mate-p-6492.html)
(XIAO ESP32-C3 + IR TX/RX), used as a native Home Assistant **Midea air-conditioner**
controller via the `midea_ir` climate platform.

One firmware (`xiao-ir-mate.yaml`) flashes every unit; `name_add_mac_suffix`
gives each node a unique hostname. The physical room is a Home Assistant **Area**,
not baked into the firmware.

## What it exposes in Home Assistant

- A `climate` entity (`midea_ir`): modes (auto / cool / dry / fan_only / heat),
  fan speeds (auto / low / medium / high), vertical swing, and the **Sleep** and
  **Boost** (turbo) presets — all stateful, held on the device.
- A `button` "AC Display Toggle" — the indoor unit's LED/display toggle, sent as a
  raw Midea IR frame (the `midea_ir` platform doesn't expose it natively).

## Secrets

Copy `secrets.yaml.example` → `secrets.yaml` (gitignored) and fill in your Wi-Fi
credentials. `ota_password` is preset to the Seeed factory OTA password so the
first over-the-air flash authenticates against the stock firmware.

## Flashing (OTA, no USB)

The device ships with ESPHome factory firmware already on Wi-Fi, so flashing is
over-the-air. Because the hostname carries a MAC suffix the build host can't
predict, target each unit by IP:

```bash
esphome run xiao-ir-mate.yaml --device 192.168.1.144   # living room
esphome run xiao-ir-mate.yaml --device <bedroom-ip>    # bedroom
```

USB-C is only a recovery fallback.

## Provisioning a new unit's Wi-Fi

A factory-fresh XIAO IR Mate raises an AP `XIAO IR Mate` (password `12345678`).
Connect, open `http://192.168.4.1`, pick your 2.4 GHz SSID. Once it joins Wi-Fi,
flash it OTA as above.
