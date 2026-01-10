# Nebulizador – ESPHome Device

Handheld nebulizer controlled via ESPHome and Home Assistant.

This device is intentionally simple and explicit.  
No hidden state, no cloud dependency, no HA helpers required.

---

## What this device does

- Exposes a **stateless command** to trigger nebulization
- Duration is configurable locally on the device
- Concurrency-safe (single run at a time)
- Fully portable across Home Assistant instances

---

## Main entities

### Button
- **Fire Nebulizer**
  - Triggers a nebulization cycle
  - Uses the current configured duration
  - Ignored if already running

### Number
- **Duration** (seconds)
  - Range: 1–300s
  - Stored locally on the device (flash)
  - Rendered as a numeric input (BOX mode)

### Binary Sensor
- **Running**
  - `ON` while nebulizer is active
  - Useful for UI state or automations

---

## API Services

The device exposes two ESPHome API services:

### `fire`
```yaml
service: esphome.nebulizador_fire
data:
  duration_s: 10
````

* Triggers nebulization for the given duration
* Duration is clamped internally (safety)

### `fire_default`

```yaml
service: esphome.nebulizador_fire_default
```

* Triggers nebulization using the current `Duration` value

---

## Concurrency model

* Internally implemented using an ESPHome `script` with `mode: single`
* While running:

  * Button presses are ignored
  * API calls are ignored
* This avoids overlapping activations and race conditions

---

## Networking & onboarding

### Wi-Fi provisioning

* Uses **Improv (BLE + Serial)** for onboarding
* Wi-Fi credentials are stored in device flash (NVS)
* YAML does **not** need Wi-Fi credentials after provisioning

---

## Debug & observability

The device exposes:

* Uptime
* Reset reason
* IP address
* Wi-Fi signal strength
* Restart button

No external logging or telemetry required.

---

## Design philosophy

* Declarative firmware
* Explicit state
* No Home Assistant helpers
* No hidden resets
* No “magic” behavior

If something happens, you can see it in logs.

---

## Notes

* This is a reference implementation, not a locked product
* The YAML is intended to be readable, modifiable, and forkable
* Feel free to refactor, inline packages, or simplify further


