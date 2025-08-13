# Step 4 — OpenPnP ↔ Klipper via TCP (Windows ↔ Ubuntu/WSL)

**Goal:** Run OpenPnP on Windows, Klipper in Ubuntu/WSL, connected over TCP.

## 2.1 Start the bridge
**Ubuntu (WSL):**
```bash
# Resolve the Klipper endpoint (pty or UDS)
DEV=$(readlink -f /tmp/printer || readlink -f /tmp/klippy_uds)
echo "$DEV"

# Start the TCP bridge (leave running)
socat -d -d -T300 TCP-LISTEN:5000,bind=127.0.0.1,reuseaddr,fork,crnl \
                  OPEN:$DEV,raw,echo=0
```

**Windows PowerShell (test):**
```powershell
$client = New-Object Net.Sockets.TcpClient("127.0.0.1",5000)
$w = New-Object IO.StreamWriter($client.GetStream()); $w.AutoFlush=$true
$r = New-Object IO.StreamReader($client.GetStream())
$w.WriteLine("M105"); Start-Sleep -Milliseconds 200
$r.ReadToEnd(); $client.Close()
# Expect:
# // Klipper state: Ready
# ok T0:xx.x /0.0
```

## 2.2 Configure the OpenPnP Gcode Driver
- **Connection:** TCP, Host `127.0.0.1`, Port `5000`, LF line ending, Timeout 2000–60000 ms.
- **Connect sequence:**
```
G90
M83
```
- **Home command:** `G28` (or `OPNP_HOME` macro, see below)
- **Get position:** `M114`
- **Position regex:**  
`(?i)X:(?<X>-?\d+\.?\d*)\s+Y:(?<Y>-?\d+\.?\d*)\s+Z:(?<Z>-?\d+\.?\d*)\s+E:(?<E>-?\d+\.?\d*)`
- **Move template (works):**
```
G1 {X:X%.4f}{Y:Y%.4f}{Z:Z%.4f}{E:E%.4f}F{FeedRate:%.0f}
```
> Tip: Enable “using-letter-variables” in the driver options.

**Optional macro (Klipper):**
```ini
[gcode_macro OPNP_HOME]
gcode:
  G28
  G90
  G92 X0 Y0 Z0 E0
  M114
```

---

### Problem → Solution
- **Problem:** Timeouts on first connect  
  **Solution:** Add `M105` to the connect sequence and raise timeout. Confirm bridge with the PowerShell round‑trip test above.

- **Problem:** Driver sends raw `{x:X...}` text to Klipper  
  **Solution:** Use the exact template above and the “using‑letter‑variables” option.

- **Problem:** PR nags about firmware detection  
  **Solution:** It’s benign if `M115` returns a Klipper line; you can quiet the warning with a static detected‑firmware string in `machine.xml`.
