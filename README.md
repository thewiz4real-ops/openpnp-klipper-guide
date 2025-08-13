# OpenPnP + Klipper on SKR Mini E3 V3 (Windows + WSL2)

**One repo. Clean steps. Commands clearly marked for Windows or Ubuntu/WSL.**  
This guide fixes common pitfalls and clarifies the *“Z shows −61”* non‑issue (camera focus vs. nozzle Z).

## Quick Start
1. **Bill of Materials:** See [10-bom.md](docs/10-bom.md)
2. **Flash/Host Setup:** See [20-skr-mini-wsl2.md](docs/20-skr-mini-wsl2.md)
**2.5** **Before Starting OpenPnP (each boot):** See [25-before-starting-openpnp.md](docs/25-before-starting-openpnp.md).
3. **OpenPnP ↔ Klipper driver (TCP bridge):** See [30-openpnp-driver.md](docs/30-openpnp-driver.md)
4. **Head rotation via fake extruder (EM/E0):** See [40-rotation-extruder.md](docs/40-rotation-extruder.md)
5. **Lights (ESP32 or SKR fans):** See [50-lights.md](docs/50-lights.md)
6. **Troubleshooting (Step → Problem → Solution):** See [60-troubleshooting.md](docs/60-troubleshooting.md)

👉 Example configs live in [`configs/`](configs/).

## Contributing
Found something to fix? Please open an Issue or a Pull Request.  
See [CONTRIBUTING.md](CONTRIBUTING.md).

## License
- Docs: **CC BY 4.0**
- Code/Configs: **MIT**

*Generated: 2025-08-13 21:21*

---

## Project Notes

- **Attribution:** This machine is **inspired by the LumenPnP open‑source project (v4)**. Most **3D printables** and the **stopper PCB** come from the LumenPnP v4 project.
- **Do not overwrite your own `machine.xml`:** The example in `configs/machine.xml` is **guidance only**. **Do not** replace your working OpenPnP `machine.xml` with it; copy settings over selectively.
- **BOM:** The bill of materials is now in **[10-bom.md](docs/10-bom.md)**.
- **Where to put the ring‑light KiCad project:** place it under **`hardware/ringlight-24v-2x4-osram/`**. (See **hardware/README.md** for folder layout.)

*Notes updated: 2025-08-13 21:35*
