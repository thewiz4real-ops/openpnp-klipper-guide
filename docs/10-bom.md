# Step 6 — Bill of Materials (BOM)

**This build is inspired by the LumenPnP v4 project.** Most 3D printables and the stopper PCB originate from LumenPnP v4.

> ⚠️ **Do not replace your OpenPnP `machine.xml` with the one in this repo.** Use `configs/machine.xml` as **guidance only** and copy settings over selectively.

---

## From AliExpress
- 1× 370 Mini Air Pump 3V–24V 2.4W 3 L/min Electric Micro Vacuum Booster Motor
- 1× SMT head motor NEMA8/NEMA11 with nozzle & air connector (JUKI500 kit, KSH04‑M5)
- 1× OpenPnP USB Interface 1.0MP 720p USB camera module (IR‑cut switch, 8 mm lens)
- 1× OpenPnP USB Interface 1.0MP 720p USB camera module (IR‑cut switch, 120° lens)
- 4× GT2 idler timing pulley 20T W6 B5
- 1× BIGTREETECH **SKR Mini E3 V3.0** (TMC2209 UART)
- 5 m × GT2 timing belt, 6 mm width
- 5 m × 3 mm ID × 5 mm OD clear hose (food grade) — for vacuum
- 1× 24 V small normally‑closed solenoid valve

## From Temu
- 4× NEMA17 stepper, ~1.0 A
- 2× 50‑pack T‑nuts **M3**
- 1× 50‑pack T‑nuts **M5**
- 1× Panel holders for 2020/3030/4040 (Euro standard T‑slot)
- 2× Nylon drag chain/towline (10×20 mm class)
- 1× 100‑pack M3‑0.5 ISO7380 button‑head (304 A2‑70)
- 1× 50‑pack slide‑in T‑nuts **M5** (20‑series)
- 1× 2020 connector set (20 corner brackets + 40 M5×8 + 40 M5 T‑nuts)
- 2× Dual Z‑axis stepper motor cables (≈1.5 m) for Ender‑3/CR‑10
- 1× XYZ stepper & endstop cable set (Ender‑3 class)
- 3× **MGN9H** linear rails 350 mm
- 2× **MGN9H** linear rails 100 mm
- 1× GT2 kit (8× 5 mm‑bore 20T pulleys + 5 m belt, 6 mm width)
- 1× Ten‑pack KSV2Z Anodized Black 2020 T‑slot profiles (~1000.8 mm each)

## Frame Cuts (from 2020 stock)
- X axis: **3 × 460 mm**
- Y axis: **4 × 420 mm**
- Bed extra reinforcement: **1 × 420 mm**

## Printables & PCBs (LumenPnP v4)
- **All 3D printed parts:** from **LumenPnP v4**.
- **Stopper PCB:** from **LumenPnP v4** as well.

## Other Plates & Modules
- **Aluminum bed:** **460 mm × 300 mm**, hole array every **30 mm**, rows offset (“staggered”), camera hole φ **45 mm**.
- **Z gantry** compatible with **JUKI nozzle** (folder provided under `hardware/`).
- **24 V ring light** — KiCad project goes under `hardware/ringlight-24v-2x4-osram/`.

## TODO / Community Help
- 4× **M2.5** screws for JUKI nozzle motor (lengths vary)
- Assorted **M3/M4/M5** screws — see assembly video for exact lengths
- If you can refine sizes, please submit a PR to update this BOM.

