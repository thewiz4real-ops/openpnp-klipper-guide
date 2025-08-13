# Step 2 — SKR Mini E3 V3 + Klipper on Windows (WSL2)

**Goal:** Flash Klipper to SKR Mini E3 V3.0 and run Klipper host in Ubuntu (WSL2) on Windows.

## Hardware & Requirements
- SKR Mini E3 V3.0 (MCU STM32G0B1)
- 24 V PSU, USB‑A→Micro‑B data cable
- Windows 10/11 with WSL2 (Ubuntu 24.04 suggested)
- `usbipd-win` for USB into WSL (optional)

## Safe Power Sequence (prevents USB/UART lockups)
1. Set **5 V jumper** to `INT`
2. Remove **SW_USB** jumper
3. Turn **VIN (24 V) ON first**
4. Then plug in **USB**

## A) Pass the board into WSL (optional)
**Windows PowerShell (Admin):**
```powershell
winget install dorssel.usbipd-win
usbipd list
usbipd bind   --busid 1-1
usbipd attach --busid 1-1 --wsl --auto-attach
```

## B) Prepare Ubuntu (WSL)
**Ubuntu (WSL):**
```bash
sudo apt update
sudo apt install -y git build-essential python3-venv nginx curl unzip socat
```

## C) Build & Flash Klipper Firmware
**Ubuntu (WSL):**
```bash
git clone https://github.com/Klipper3d/klipper
cd klipper
make menuconfig
# MCU: STMicroelectronics STM32
# Processor: STM32G0B1
# Bootloader offset: 8KiB bootloader
# Interface: USB (PA11/PA12)
make
```
**Flash via SD (recommended):**
1. Copy `out/klipper.bin` to SD, rename to `firmware.bin`
2. Insert in SKR, power cycle
3. Green LED blinks while flashing; file becomes `FIRMWARE.CUR`

## D) Install Klipper & Moonraker services
**Ubuntu (WSL):**
```bash
# Klipper host
python3 -m venv ~/klippy-env
~/klippy-env/bin/pip install -r klipper/scripts/klippy-requirements.txt
sudo cp klipper/scripts/klipper.service /etc/systemd/system/
sudo sed -i "s;/home/pi;/home/$USER;g" /etc/systemd/system/klipper.service
sudo systemctl enable --now klipper

# Moonraker
git clone https://github.com/Arksine/moonraker ~/moonraker
python3 -m venv ~/moon-env
~/moon-env/bin/pip install -r ~/moonraker/scripts/requirements.txt
sudo cp ~/moonraker/scripts/moonraker.service /etc/systemd/system/
sudo sed -i "s;/home/pi;/home/$USER;g" /etc/systemd/system/moonraker.service
sudo systemctl enable --now moonraker
```

## E) Mainsail (UI) via nginx
**Ubuntu (WSL):**
```bash
mkdir -p ~/mainsail
curl -L https://github.com/mainsail-crew/mainsail/releases/latest/download/mainsail.zip -o mainsail.zip
unzip mainsail.zip -d ~/mainsail
sudo mkdir -p /var/www && sudo cp -r ~/mainsail /var/www/

sudo tee /etc/nginx/sites-available/mainsail >/dev/null <<'EOF'
server {
    listen 80 default_server;
    server_name _;
    root /var/www/mainsail;
    index index.html;
    location / { try_files $uri $uri/ @moonraker; }
    location @moonraker {
        proxy_pass http://127.0.0.1:7125;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
    }
}
EOF
sudo ln -sf /etc/nginx/sites-available/mainsail /etc/nginx/sites-enabled/
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t && sudo systemctl restart nginx
```

## F) Find your MCU path
**Ubuntu (WSL):**
```bash
ls -l /dev/serial/by-id/
# copy usb-Klipper_stm32g0b1xx_...-if00
```

## G) Minimal `printer.cfg` (motion only)
See [`configs/printer.cfg`](../configs/printer.cfg).

---

### Problem → Solution
- **Problem:** 404 at `http://localhost/`  
  **Solution:** Ensure nginx site enabled & Moonraker running; re-run the nginx block above.

- **Problem:** MCU not found  
  **Solution:** Use `/dev/serial/by-id/...` path and confirm with `ls -l`. Ensure power sequence.
