# Step 3 — Head Rotation on EM/E0 as Fake Extruder (Klipper)

**Goal:** Drive the head rotation motor from the SKR Mini E3 V3 EM/E0 driver using Klipper’s `[extruder]` so we get TMC current control.

## 3.1 Clean out old attempts
Remove previous blocks like:
```
[manual_stepper head_rot]
[stepper_generic head_rot]
[tmc2209 head_rot]
# or old [extruder] attempts
```

## 3.2 Config (example)
```ini
# --- PnP head rotation on EM driver (fake extruder) ---
[extruder]
step_pin: PB3
dir_pin: PB4              # add "!" if direction is backwards
enable_pin: !PD1
microsteps: 16
rotation_distance: 360    # 1 unit = 1°
nozzle_diameter: 0.4
filament_diameter: 1.75
heater_pin: PA1
sensor_type: temperature_mcu
control: watermark
min_temp: 0
max_temp: 999
min_extrude_temp: 0
max_extrude_only_distance: 100000
max_extrude_only_velocity: 200

[tmc2209 extruder]
uart_pin: PC11
tx_pin:  PC10
uart_address: 3
run_current: 0.45
hold_current: 0.10
stealthchop_threshold: 999
```

## 3.3 Test
**Ubuntu (WSL) → Mainsail console:**
```
RESTART
SET_STEPPER_ENABLE STEPPER=extruder ENABLE=1
DUMP_TMC STEPPER=extruder   # IOIN ... enn=0 means enabled
M83
G92 E0
G1 E90 F600   ; +90°
G1 E-90 F600  ; back
```
If backwards → `dir_pin: !PB4` and `RESTART`.

## 3.4 Macros
```ini
[gcode_macro ROT]
# ROT D=90  (relative degrees)
gcode:
  {% set d = params.D|float %}
  M83
  G1 E{d} F600

[gcode_macro ROTABS]
# ROTABS A=0  (absolute angle)
variable_pos: 0
gcode:
  {% set tgt = params.A|float %}
  {% set delta = tgt - printer["gcode_macro ROTABS"].pos %}
  SET_GCODE_VARIABLE MACRO=ROTABS VARIABLE=pos VALUE={tgt}
  M83
  G1 E{delta} F600
```

---

### Problem → Solution
- **Problem:** Locked but won’t move  
  **Solution:** Wrong STEP/DIR pins or still in absolute E (send `M83` or `G92 E0`).

- **Problem:** Hot driver at idle  
  **Solution:** Lower `hold_current`, or disable after idle with an `idle_timeout` macro.

- **Problem:** “Move exceeds maximum extrusion”  
  **Solution:** Either (a) send rotation on a separate `G1 E...` line, or (b) relax `[extruder]` limits (we already set safe high limits above).
