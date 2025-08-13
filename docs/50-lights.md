# Step 5 — Lights (ESP32 serial **or** SKR fan headers)

Pick one approach.

## Option A — ESP32 over Serial (Windows COMx)
- **Windows (OpenPnP):** Add a Serial driver (e.g., COM4 @ 115200, keep‑alive).  
  **Actuators mapping:**
  - `LIGHT_Z` → `{True:z on}{False:z off}`
  - `LIGHT_FLAT` → `{True:f on}{False:f off}`
- **Tip:** Close Arduino Serial Monitor or any app that might hold the COM port.

### Problem → Solution
- **Problem:** “timeout waiting for response”  
  **Solution:** Ensure only OpenPnP holds the COM port; raise timeout to ~5000 ms; unplug/replug device if Windows power‑saves the port.

## Option B — SKR Fan Headers (24 V LED rings)
**Klipper (Ubuntu/WSL):**
```ini
[fan_generic light_flat]
pin: PC7           # FAN1
cycle_time: 0.0005

[fan_generic light_z]
pin: PB15          # FAN2
cycle_time: 0.0005
```
**OpenPnP Boolean actuators:**  
`SET_FAN_SPEED FAN=<name> SPEED={0..1}`

### Problem → Solution
- **Problem:** Visible PWM banding in camera  
  **Solution:** Use small `cycle_time` (~0.0005 ≈ ~2 kHz) and/or adjust exposure.
