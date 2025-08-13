# Step 2.5 (Boot) — Before Starting OpenPnP (Windows + Ubuntu/WSL)

**Purpose:** These commands are the *per‑boot* steps you run **every time** you start Windows **before** launching OpenPnP.
They are **not** part of the installation process.

---

## What you’ll do each time

1. **Open two consoles**  
   - One **Windows** (Command Prompt or PowerShell)  
   - One **Ubuntu (WSL)**

2. **Windows console — attach the SKR to WSL**  
   Replace `1-1` with the BUSID shown by `usbipd list` for your SKR Mini:
   ```powershell
   usbipd attach --busid 1-1 --wsl -a
   ```
   > Tip: If you haven’t installed it yet (one time only): `winget install dorssel.usbipd-win`

3. **Ubuntu (WSL) console — start the TCP bridge**  
   Resolve Klipper’s printer endpoint and start `socat`. Leave this window running.
   ```bash
   # Resolve the real endpoint Klipper created
   DEV=$(readlink -f /tmp/printer || readlink -f /tmp/klippy_uds)
   echo "$DEV"

   # Bridge Windows <-> WSL (OpenPnP will connect to TCP 127.0.0.1:5000)
   socat -d -d -T300 TCP-LISTEN:5000,bind=127.0.0.1,reuseaddr,fork,crnl \
                  OPEN:$DEV,raw,echo=0
   ```

4. **(Optional) Windows test — confirm round‑trip**  
   From **Windows PowerShell**, verify Klipper answers `ok`:
   ```powershell
   $client = New-Object Net.Sockets.TcpClient("127.0.0.1",5000)
   $w = New-Object IO.StreamWriter($client.GetStream()); $w.AutoFlush=$true
   $r = New-Object IO.StreamReader($client.GetStream())
   $w.WriteLine("M105"); Start-Sleep -Milliseconds 200
   $r.ReadToEnd(); $client.Close()
   ```

5. **Start OpenPnP** (Windows) and connect the G‑code driver to **Host: `127.0.0.1` / Port: `5000`**.

---

## Stop / restart
- To stop the bridge, close the WSL window running `socat` (or `pkill -f socat`).  
- If OpenPnP disconnects, just restart step 3 and reconnect the driver.

## Common pitfalls
- **`Connection refused`**: Klipper didn’t create `/tmp/printer` yet → wait a few seconds or restart the Klipper service.  
- **Timeouts on first command**: Add `M105` to the driver “connect” sequence and raise timeout.  
- **Wrong BUSID**: Run `usbipd list` in Windows to find the right `--busid` for the SKR.

*Last updated: 2025-08-13 22:20*

