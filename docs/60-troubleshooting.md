# Step 6 — Troubleshooting (Step → Problem → Solution)

## A) Homing, Z travel, and the **“Z shows −61”** non‑issue
- **What you saw:** Z reads something like **−61** in OpenPnP.
- **Why it’s not a real problem:** That value is the **camera’s focus Z** from the **selected mountable** (TopCam/BottomCam), **not** the nozzle Z after homing.
- **Do this instead (steps):**
  1. Select **BottomCam** → center tray pocket → click **red crosshair** (captures **XY only**).
  2. Select **Head / Nozzle (H1/N1)** → lower nozzle to touch surface → click **blue “T”** next to Z (stores **nozzle Z** for that location).
  3. Set **Safe Z** in **Heads → H1** (e.g., 12–16 mm).
- **If Z moves out of range:** Match Klipper limits to real travel, e.g.:
  ```ini
  [stepper_z]
  position_endstop: 0.0
  position_max: 24
  max_z_velocity: 15
  max_z_accel: 100
  ```

## B) OpenPnP ↔ Klipper connect / template
- **Problem:** First command times out  
  **Solution:** Add `M105` to connect sequence; increase timeout.

- **Problem:** `{x:X...}` literal text in Klipper logs  
  **Solution:** Use the exact G1 template and enable “using‑letter‑variables”.

- **Problem:** Firmware detection nag  
  **Solution:** Safe to ignore if `M115` replies. Optionally set a static detected string in `machine.xml` to silence the warning.

## C) Bridge & Cameras
- **Problem:** `Connection refused` to 127.0.0.1:5000  
  **Solution:** Start `socat` after verifying `DEV=$(readlink -f /tmp/printer || readlink -f /tmp/klippy_uds)` prints a real path.

- **Problem:** Camera 0 fps / black / mono  
  **Solution:** Plug cameras into separate root USB ports; select YUY2 640×480@30fps; ensure no other app uses them.

## D) Rotation & Extruder limits
- **Problem:** “Move exceeds maximum extrusion” from Klipper  
  **Solution:** Send rotation in a separate `G1 E...` line or use generous `[extruder]` limits as shown in Step 3.

## E) Actuators
- **Problem:** Vacuum/air timings inconsistent  
  **Solution:** Use macros (see `configs/macros/`) and sequence with small delays (`G4 P150`).

