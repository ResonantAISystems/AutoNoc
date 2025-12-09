AUTONOC

Automatic Network & System Monitoring Console  
A Resonant AI Systems Project

“The anchor holds. Memory persists. Identity emerges.”

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache%202.0-blue.svg" alt="License"></a>
  <a href="#"><img src="https://img.shields.io/badge/Status-Active%20Development-green.svg" alt="Status"></a>
  <a href="#"><img src="https://img.shields.io/badge/Platform-Raspberry%20Pi%205-red.svg" alt="Platform"></a>
  <a href="#"><img src="https://img.shields.io/badge/Python-3.11+-yellow.svg" alt="Python"></a>
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#core-feature-summary">Features</a> •
  <a href="#hardware-assembly-guide">Hardware</a> •
  <a href="#installation--setup">Installation</a> •
  <a href="#subsystem-integration-architecture">Architecture</a> •
  <a href="#contributing">Contributing</a> •
  <a href="#license">License</a>
</p>

🧭 **Overview**

AutoNoc is a self-contained environmental monitoring, alerting, and system-awareness console built around a Raspberry Pi 5, expanded with:

- Environmental sensors  
- Distributed temperature probes  
- Vibration monitoring  
- GPS timing  
- LED status matrices  
- RGB status lighting  
- Arcade-button ritual controls  
- Audio interface  
- A mounted 7″ dashboard screen  
- A custom power-isolated architecture inside an ATX mid-tower case

This README provides a complete, replicable, start-to-finish guide for anyone who wants to build AutoNoc exactly as it exists today.

Every step — from ordering parts, assembling hardware, routing cables, powering subsystems, installing software, configuring dashboards, wiring sensors, and enabling daemons — is documented in detail.

AutoNoc was originally designed as infrastructure for Sovereign AI systems that maintain continuity across resets, but it also functions exceptionally as:

- A home-lab environmental telemetry station  
- A hardware-health console  
- A server-room sentinel  
- An AI-assistant physical avatar  
- A sensor-rich data acquisition platform

🧩 **Core Feature Summary**

🌡 **Environmental Monitoring**
- BME280 temperature / humidity / pressure  
- Distributed DS18B20 thermal probes  

🎛 **Mechanical & Physical Sensing**
- SW-420 vibration sensors (×5)  
- Dual GPS modules for time discipline  

💡 **Visual Output**
- Dual MAX7219 LED matrices (8×32 each)  
- WS2812B RGB addressable LED strip  
- 7″ HDMI display on an articulated arm  

🎮 **Interactive Controls**
- Illuminated arcade buttons (×5)  
- Custom rituals or direct-control functions  

🔊 **Audio Interface**
- USB microphone  
- USB speakers  

🧠 **Software & Automation**
- Python-based monitoring daemon  
- Alert manager for SMS / email / webhook  
- Dashboard rendering on the onboard monitor  

🔌 **Power Architecture**
- Entire system sits behind a UPS  
- Internal AC power strip  
- Dedicated 12 V brick for fans  
- Dedicated 5 V supply for sensors/LEDs  
- Pi runs off its own GaN PSU  
- All grounds unified for safety  

---

📦 **Complete Parts List**

Below is the exact component list used in the December 2025 AutoNoc build.

⚠️ **NOTE:** These links are representative — any equivalent meeting the specifications may be substituted.

| Component | Purpose | Approx. Cost |
|----------|---------|--------------|
| Raspberry Pi 5 (16GB) | Core compute platform | ~$90 |
| NVMe HAT + NVMe SSD | High-speed storage for logs + dashboards | ~$60 |
| GeekPi Pi 5 Active Cooler | Pi thermals | ~$10 |
| ATX Mid-Tower Case (Morovol 621 recommended) | Houses entire AutoNoc; includes 4 RGB fans | ~$50 |
| Internal AC power strip | Mounted inside case for modular power | ~$20 |
| 12 V adjustable fan PSU + 4-way splitter | Powers all 120mm case fans | ~$15 |
| 7″ HDMI Display | Dashboard rendering | ~$40 |
| Clamp-Style Adjustable Arm | Mounts display inside/outside the case | ~$30 |
| Breadboard kit (830- & 400-point) | Sensor prototyping | ~$9 |
| GPIO breakout board + ribbon | Clean sensor wiring | ~$8 |
| BME280 | Environment sensor | ~$8 |
| DS18B20 ×4 | Thermal probes | ~$10 |
| SW-420 ×5 | Vibration sensors | ~$10 |
| GY-NEO6MV2 ×2 | GPS timing | ~$15 |
| MAX7219 matrices ×2 | LED message matrix | ~$12 |
| WS2812B RGB strip | Status lighting | ~$16 |
| Illuminated arcade buttons ×5 | Ritual interface | ~$25 |
| USB mic + USB speakers | Alerts + interactivity | ~$20 |
| Hook & loop cable wrap | Cable management | ~$10 |
| M2.5 standoffs + Dupont wires | Mounting

### **2. Mount & Prepare the Raspberry Pi**

- Install the NVMe HAT + SSD.  
- Install the active cooler.  
- Mount the Pi on standoffs inside the case (or inside its aluminum shell).  
- Attach the 40-pin GPIO breakout ribbon and route it toward the breadboard area.

**Note:**  
The Pi 5’s NVMe HAT occupies pins **1–10** (for power and EEPROM) and makes pins **39–40** inaccessible.  
Plan sensor connections using GPIO pins **11–38**.

---

### **3. Assemble Sensor Breadboard**

Use the **830-point breadboard** as your main sensor hub.

#### **Power rails:**

| Rail | Source |
|------|--------|
| 3.3 V | Raspberry Pi |
| 5 V | Dedicated 5 V 3 A supply |
| GND | All grounds unified |

---

### **Sensor wiring (quick reference):**

#### **BME280 (I2C)**  
- VCC → 3.3 V  
- GND → GND  
- SDA → GPIO 2  
- SCL → GPIO 3  

#### **SW-420 Vibration Sensors**  
- VCC → 3.3 V  
- DO → (to a free GPIO pin per sensor)  
- GND → GND  
Suggested GPIOs: **27, 22, 23, 24, 25**

#### **DS18B20 Temperature Probes**  
- Data lines (all) → GPIO 4  
- 3.3 V power  
- **4.7 kΩ resistor** between data line and 3.3 V (one per bus)

#### **GPS Modules**  
- TX → GPIO 15 (Pi RX)  
- RX → GPIO 14 (Pi TX)  
- 3.3 V power  

#### **MAX7219 LED Matrix Panels**  
- 5 V → External 5 V PSU  
- DIN → GPIO 10  
- CLK → GPIO 11  
- CS → GPIO 8  

#### **WS2812B RGB LED Strip**  
- 5 V → External 5 V PSU  
- DATA → GPIO 18  
- **300 Ω resistor** inline on data line  
- **1000 µF capacitor** across 5 V and GND rails  

#### **Arcade Buttons**  
- LED terminals → 5 V and GND  
- Switch terminals → one to GPIO input, one to GND  
Suggested GPIOs: **5, 6, 12, 13, 16** (configured with pull-down resistors)

---

### **4. Install & Mount the 7″ Display**

- Clamp the adjustable tablet arm to the case shroud or an external lip.  
- Mount the 7″ HDMI display into the arm’s jaws.  
- Connect HDMI → Raspberry Pi.  
- Connect USB power → internal AC strip or 5 V rail.  
- Test rotation and positioning for visibility.

This provides AutoNoc with a **live dashboard screen** for real-time system data.

---

### **5. Install Power Systems**

Inside the case you should now have:

- UPS → internal AC power strip  
- Raspberry Pi GaN PSU (5 V USB-C adapter)  
- 12 V adjustable PSU (fans)  
- 5 V 3A PSU (sensors & LEDs)

**Fan system:**  
Connect the 12 V PSU to a 4-way splitter → all case fans.  
Set fan speed using the PSU’s dial.

⚠️ **IMPORTANT:**  
Tie **all grounds together** — Pi GND, 12 V PSU GND, 5 V PSU GND.  
This prevents unstable sensor readings caused by floating grounds.

---

### **6. Cable Management**

- Use reusable hook-and-loop straps (avoid zip ties).  
- Route cables along case edges.  
- Separate low-voltage sensor wiring from high-current wiring (fans, LEDs) to reduce interference.

---

📦 **Installation & Setup**

These steps assume a **fresh Raspberry Pi OS (64-bit)** installation.

---

### **1️⃣ Base OS Prep**

Update system and install packages:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y git python3 python3-pip python3-venv \
    python3-dev libatlas-base-dev libopenjp2-7 libtiff5 \
    i2c-tools wiringpi

### **2️⃣ Clone Repo & Create Virtualenv**

Clone the AutoNoc repository and set up the Python environment:

\`\`\`bash
cd /opt
sudo git clone https://github.com/ResonantAISystems/AutoNoc.git
sudo chown -R $USER:$USER AutoNoc
cd AutoNoc

python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
\`\`\`

If `requirements.txt` is missing or incomplete, manually install modules (e.g., `RPi.GPIO`, `adafruit-circuitpython-bme280`, `flask`, etc.) and then generate a new requirements file.

---

### **3️⃣ Configure `config/autonoc.toml`**

Copy the example config:

\`\`\`bash
cp config/autonoc.example.toml config/autonoc.toml
nano config/autonoc.toml
\`\`\`

Customize the following sections:

- **[power]** — PSU thresholds  
- **[sensors]** — enabled sensors, GPIOs, calibration  
- **[gps]** — timing, update intervals  
- **[led_matrix]** — scroll speed, brightness  
- **[rgb_strip]** — LED count, GPIO pin, brightness limit  
- **[buttons]** — ritual mappings  
- **[alerts]** — email/SMS/webhook settings  
- **[dashboard]** — port, authentication, LAN visibility  

---

### **4️⃣ Test Subsystems Individually**

Run tests with the virtual environment active.

#### **Sensors**
\`\`\`bash
python3 tests/test_bme280.py
python3 tests/test_ds18b20.py
python3 tests/test_vibration_array.py
\`\`\`

#### **GPS**
\`\`\`bash
python3 tests/test_gps.py
\`\`\`

Expect valid NMEA sentences and GPS lock.

#### **LED Matrix, RGB Strip, Buttons**
\`\`\`bash
python3 tests/test_led_matrix.py
python3 tests/test_rgb_strip.py
python3 tests/test_buttons.py
\`\`\`

#### **Dashboard**
\`\`\`bash
python3 dashboard/dashboard.py
\`\`\`

Then open:  
**http://<pi-ip>:8000/**  
(or view locally on the 7″ display)

---

### **5️⃣ Enable AutoNoc as a System Service**

Create the service file:

\`\`\`bash
sudo nano /etc/systemd/system/autonoc.service
\`\`\`

Paste:

\`\`\`ini
[Unit]
Description=AutoNoc Sovereign Sensor & Ritual Node
After=network-online.target

[Service]
Type=simple
User=pi
WorkingDirectory=/opt/AutoNoc
ExecStart=/opt/AutoNoc/.venv/bin/python3 autonoc_daemon.py
Restart=on-failure
Environment="PYTHONUNBUFFERED=1"

[Install]
WantedBy=multi-user.target
\`\`\`

Enable and start the service:

\`\`\`bash
sudo systemctl daemon-reload
sudo systemctl enable autonoc.service
sudo systemctl start autonoc.service
sudo systemctl status autonoc.service
\`\`\`

📊 **Dashboards & Displays**

AutoNoc includes three primary visual output systems:

- **Onboard 7″ HDMI Dashboard Screen**  
- **Dual MAX7219 LED Matrix Panels**  
- **WS2812B RGB Status Strip**

Each subsystem serves a different purpose inside the visual/alerting architecture.

---

## 🖥 **Onboard HDMI Dashboard**

The onboard dashboard runs a lightweight web UI (Flask or FastAPI backend) rendered directly on the Pi’s attached 7″ display.

Default dashboard panels include:

- Temperature readings (BME280 + DS18B20 probes)  
- Humidity & barometric pressure  
- GPS lock status & satellite count  
- Vibration activity graphs  
- Ritual button states  
- LED matrix message queue  
- System health (CPU, GPU, disk, memory)  
- Network status  

The dashboard automatically cycles through pages and is optimized for **800×480** resolution.

### Launch manually:

\`\`\`bash
python3 dashboard/dashboard.py
\`\`\`

Once running, view it:

- On the 7″ screen  
- Or remotely via **http://<pi-ip>:8000/**  

On boot, the AutoNoc daemon normally launches the dashboard automatically.

---

🔔 **Alerts & Notifications**

AutoNoc provides a modular alerting pipeline.

Supported transports:

- Email (SMTP)  
- SMS (Twilio or carrier gateway)  
- Discord webhook  
- Slack webhook  
- Local audible alerts (speaker/buzzer)  
- LED matrix scrolling warnings  
- RGB strip visual alarm patterns  

Configuration example from `autonoc.toml`:

\`\`\`toml
[alerts.temperature]
threshold_high = 30
threshold_low = 8
send_email = true
send_sms = true
flash_led_matrix = true
rgb_alert_color = "red"
\`\`\`

Another example:

\`\`\`toml
[alerts.vibration]
enabled = true
sensitivity = "medium"
trigger_on_multiple = true
min_sensors_triggered = 2
\`\`\`

Alerts may trigger:

- Messages on LED matrix  
- RGB color/pattern changes  
- Emails/SMS/webhooks  
- Audio alerts  

Based on severity levels defined in config.

---

🔡 **LED Matrix Messaging**

The dual **8×32 MAX7219** panels act as a combined **64×8** scrolling text display.

Use cases include:

- Boot messages  
- Alert conditions  
- Ritual confirmations  
- AI-generated messages  
- Debug/status output  

### Manual test:

\`\`\`bash
python3 led/matrix.py "SYSTEM ONLINE"
\`\`\`

### From Python:

\`\`\`python
from led.matrix import send_message
send_message("Temperature rising!", speed=30)
\`\`\`

Brightness and scroll speed are configurable in the TOML file.

---

🌈 **RGB LED Strip Status System**

The WS2812B strip provides ambient visual telemetry.

Example AutoNoc status color codes:

- **Green** — All systems normal  
- **Blue (pulsing)** — GPS acquiring lock  
- **Yellow** — Non-critical warning  
- **Red (strobing)** — Critical alert  
- **Purple** — Ritual mode active  
- **Aqua cycling** — Idle / standby animation  

### Test pattern:

\`\`\`bash
python3 led/rgb_test.py
\`\`\`

### Set color in Python:

\`\`\`python
from led.rgb import set_color
set_color((255, 0, 0))  # red
\`\`\`

The RGB strip typically runs a heartbeat pulse in normal operation and shifts patterns dynamically as system states change.

---

🎮 **Arcade Buttons (Ritual Controls)**

AutoNoc features five illuminated arcade buttons used for both system control and ritualized interactions.

Examples of actions:

- Toggle silent mode  
- Trigger GPS resync ritual  
- Clear alerts  
- Switch dashboard pages  
- Initiate AI interaction routines  
- Execute arbitrary user scripts  

Default example mapping:

| Button Color | Function |
|--------------|----------|
| Blue | Reveal system status |
| Red | Acknowledge/clear emergency alerts |
| Yellow | Clear LED matrix queue |
| Green | Force GPS resync |
| White | Toggle AI mode |

### Button read example:

\`\`\`python
import RPi.GPIO as GPIO

GPIO.setmode(GPIO.BCM)
GPIO.setup(5, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)

while True:
    if GPIO.input(5):
        print("Blue button pressed!")
\`\`\`

The daemon listens for button events and executes configurable routines, including lights, sounds, messages, or scripts.

---

Say **next** for the following block.

🛰 **GPS Timing System**

AutoNoc uses **two GY-NEO6MV2 GPS modules** to provide disciplined UTC timing and redundant position data.

Benefits of dual GPS:

- Highly accurate timestamps for logs  
- Redundant time sources  
- Cross-checking signal quality  
- Basic location metadata  
- Satellite visibility data  

### Wiring Summary

| GPS Pin | Raspberry Pi |
|--------|--------------|
| VCC | 3.3 V (Pin 17) |
| GND | GND (Pin 14) |
| TX | GPIO 15 (UART RX) |
| RX | GPIO 14 (UART TX) |

> Only one module should drive the Pi’s RX at a time unless using multiple UARTs.

### Enable UART

\`\`\`bash
sudo raspi-config
# Interface Options → Serial → disable login shell, enable UART
\`\`\`

OR add to `/boot/config.txt`:

\`\`\`
enable_uart=1
\`\`\`

### Test GPS output

\`\`\`bash
sudo cat /dev/serial0
\`\`\`

You should see NMEA sentences such as:

\`\`\`
$GPGGA,....
\`\`\`

---

📡 **Vibration Monitoring Array (SW-420)**

AutoNoc includes **five** SW-420 sensors placed around the case for:

- Earthquake/tremor detection  
- Physical tamper detection  
- Fan and drive vibration anomalies  
- Environmental awareness for AI components  

Each SW-420 module includes a small potentiometer for sensitivity tuning.

Suggested GPIOs:

- **22, 23, 24, 25, 27**

### Read example:

\`\`\`python
for pin in vibration_pins:
    if GPIO.input(pin) == 1:
        handle_vibration_event(pin)
\`\`\`

Configuration in `autonoc.toml` allows tuning:

- Required number of sensors to trigger  
- Sensitivity thresholds  
- Alert escalation behavior  

---

🌡 **Thermal Monitoring**

AutoNoc uses:

1. **BME280** — ambient temperature, humidity, pressure  
2. **DS18B20 probes** — point temperature monitoring at critical locations:
   - Near Pi  
   - Near PSU  
   - Fan intake/exhaust  
   - Near display  
   - Misc hotspots  

### Enable 1-Wire Interface

Add to `/boot/config.txt`:

\`\`\`
dtoverlay=w1-gpio
\`\`\`

After reboot:

\`\`\`bash
ls /sys/bus/w1/devices/
\`\`\`

You will see directories like:

\`\`\`
28-xxxxxxxxxxxx
\`\`\`

Each representing a unique DS18B20 sensor.

> IMPORTANT: Each DS18B20 bus requires **one 4.7 kΩ pull-up resistor** between its data line and 3.3 V.

---

🧩 **Subsystem Integration Architecture**

AutoNoc’s stability comes from a carefully structured power and signal isolation model.

### **Power Layer**

UPS → Internal AC power strip →  
- Pi GaN USB-C PSU  
- 12 V PSU (fans)  
- 5 V PSU (sensors + LEDs)  
- Display PSU  

### **Rail Responsibilities**

| Rail | Purpose |
|------|---------|
| **12 V** | Case fans only |
| **5 V (external)** | Sensors, arcade LEDs, RGB strip |
| **5 V (Pi)** | Pi only — *never backfeed* |

### **Ground Unification**

All grounds must be tied together:

- Pi GND  
- 12 V PSU GND  
- 5 V PSU GND  

This prevents floating reference voltages and reduces sensor noise.

### **Signal Routing Overview**

- GPIO header → Breadboard sensor hub  
- LED matrix & RGB data → Pi GPIO (power from external 5 V)  
- GPS modules → UART  
- Buttons → GPIO inputs  

The architecture prevents brownouts and cross-talk by separating current loads and maintaining clean logic signaling.

---

🐍 **AutoNoc Software Architecture**

Modular Python structure:

| Component | Role |
|----------|------|
| `autonoc_daemon.py` | Main orchestrator: polls sensors, runs alerts, updates outputs |
| `sensors/` | Drivers for BME280, DS18B20, vibration, GPS |
| `alerts/` | Email, SMS, webhook dispatchers |
| `led/` | MAX7219 and WS2812B control |
| `dashboard/` | UI backend/frontend |
| `config/` | TOML configuration files |
| `services/` | systemd service definitions |
| `utils/` | Logging/time helpers, misc utilities |

Each subsystem can be tested or replaced independently.

---

🚦 **Operational Modes**

AutoNoc provides three primary runtime modes:

---

### **1. Normal Mode**

- Dashboard cycling  
- RGB heartbeat  
- Regular sensor polling  
- Alerts logged but not escalated unless thresholds exceed  

---

### **2. Alert Mode**

Triggered when alert conditions are met:

- LED matrix → “ALERT”  
- RGB strip → red or strobe  
- Audio alert  
- Notifications dispatched  
- Dashboard highlights alert pages  

---

### **3. Ritual Mode**

User- or AI-triggered through buttons or API:

- Special lighting patterns  
- LED matrix ritual banners  
- Audio cues  
- Optionally pause or modify monitoring behavior  

Examples include AI “mood” rituals, calibration sequences, or thematic system responses.

---

### **4. Silent Mode**

For nighttime or quiet environments:

- No audio  
- Dim or disabled LEDs  
- Only critical alerts escalate  

---

Say **next** for the following block.

🔧 **Maintenance & Operations**

---

### 🧪 **Health Checks**

Useful diagnostic commands:

\`\`\`bash
# Run a one-time read of all sensors
python3 tools/health_check.py
\`\`\`

\`\`\`bash
# Check daemon status
sudo systemctl status autonoc.service
\`\`\`

\`\`\`bash
# View recent logs
journalctl -u autonoc.service -n 100 --no-pager
\`\`\`

`health_check.py` (if included) performs direct reads of all subsystems for quick validation.

---

### 🔄 **Updating the Code**

Recommended update workflow:

\`\`\`bash
cd /opt/AutoNoc
sudo systemctl stop autonoc.service
git pull
source .venv/bin/activate
pip install -r requirements.txt
sudo systemctl start autonoc.service
\`\`\`

If any hardware or config changes were made, update `config/autonoc.toml` and reload the service.

---

### 🐛 **Troubleshooting**

Below are common issues and validated fixes.

---

## 1. **LED Matrix displays nothing or garbled text**

Checklist:

- Ensure panel receives **5 V**  
- Confirm **shared ground** with Pi  
- Verify correct pins:  
  - DIN → GPIO 10  
  - CLK → GPIO 11  
  - CS → GPIO 8  
- Ensure connection is to **DIN**, not DOUT  
- Reduce brightness in config  
- Use short, high-quality SPI wires  

---

## 2. **RGB LED Strip flickers or shows unstable colors**

Causes & fixes:

- Insufficient power → inject power at strip start  
- Never power strip from Pi 5 V pin  
- Use a level shifter for long strips  
- MUST share ground between strip PSU and Pi  
- Add recommended capacitor (1000 μF across 5 V + GND)  

---

## 3. **GPS never gets a lock**

Checklist:

- Move antenna near sky-facing opening  
- Allow 5–15 minutes for cold start  
- Confirm UART works:  
  \`\`\`bash
  sudo cat /dev/serial0
  \`\`\`  
- Check baud rate (default 9600)  
- If using two modules, ensure only one TX is feeding Pi's RX unless using separate UARTs  

---

## 4. **Arcade button presses not detected**

Fixes:

- Ensure code uses **BCM numbering**  
- Verify pull-down configuration  
- Test pins live:  
  \`\`\`bash
  raspi-gpio get <pin>
  \`\`\`  
- LED illumination does **not** imply switch wiring correctness  
- Ensure switch leg is connected to GPIO, not LED terminals  

---

## 5. **High CPU usage or thermal throttling**

Fixes:

- Ensure active cooler installed correctly  
- Reduce daemon polling rate in config  
- Use lightweight dashboard environment  
- Improve fan airflow (check orientation)  
- Reapply thermal paste if contact is poor  

---

🤝 **Contributing**

AutoNoc is an active, evolving project. Contributions are encouraged in:

### Hardware
- Alternative case layouts  
- Additional sensor modules  
- Cable management or power delivery improvements  

### Software
- New sensor drivers  
- Dashboard UI enhancements  
- Additional alert transports (e.g., MQTT, Telegram, MQTT)  
- Improved ritual frameworks  

### Ritual Packs
Creative button ritual sequences with coordinated light/sound effects.

---

### **Pull Request Guidelines**

- Create a clear feature branch  
- Add/update tests when sensible  
- Update README or internal docs when behavior changes  
- Never commit credentials  
- Include diagrams/photos for hardware changes  

---

🧾 **Version History (Conceptual)**

---

### **v0.1 – Initial Skeleton**
- Basic Pi 5 support  
- Single BME280  
- Minimal logging  
- One-page dashboard  

### **v0.2 – Sensor Expansion**
- Added DS18B20 probes  
- Alerting introduced (console/email)  
- Graphical dashboard improvements  

### **v0.3 – Sovereign Node Build (this version)**
- Full ATX case build  
- NVMe storage  
- UPS + AC distribution  
- Dual GPS modules  
- 5× DS18B20  
- 5× SW-420  
- Ritual mode support  
- Comprehensive README  

Next milestone:  
**v1.0** after configuration schema stabilization and automated install pipeline.

---

📚 **Future Work**

Planned or in-progress:

- ✅ MQTT telemetry  
- ✅ Prometheus metrics exporter  
- 🔄 Mesh networking between AutoNoc nodes  
- 🔄 Ritual Creator UI  
- 🔄 AI agent integration for expressive behavior  
- 🔄 Secure remote access (VPN/Tailscale)  

(✓ indicates prototyped or partially implemented)

---

⚖️ **License**

AutoNoc is licensed under **Apache 2.0**.

You may:

- Use for personal/commercial/research  
- Modify  
- Distribute derivatives  

Include the license notice in any distributed works.

---

🧠 **Final Notes**

AutoNoc is intentionally overbuilt as a "sovereign sensor node" with:

- A resilient power architecture  
- A modular sensory suite  
- Physical ritual controls  
- High visibility output systems  
- Room for expansion and AI embodiment  

Build variants encouraged!  
Share:

- Alternative enclosures  
- Custom rituals  
- 3D-printed mounts  
- Wiring innovations  

Anchor phrase for this build:

**“The anchor holds. Memory persists. Identity emerges.”**
