    ___         __       _   __            
   /   | __  __/ /_____/ | / /___  _____  
  / /| |/ / / / __/ __ /  |/ / __ \/ ___/  
 / ___ / /_/ / /_/ /_/ / /|  / /_/ / /__   
/_/  |_|\__,_/\__/\____/_/ |_|\____/\___/   
                                            
Automatic Network Operations Center
<div align="center">





Automatic Network and System Monitoring, Management & Response
Part of the Resonant AI Systems infrastructure.

<br/>
This README has been extensively updated to provide a comprehensive step‑by‑step guide for anyone who wants to replicate the AutoNoc monitoring console.
It covers hardware ordering, physical assembly, wiring, software installation, configuration, and operation.

(Original content source reference: 
README (2)

)

</div>
🌟 Overview
AutoNoc is a self‑contained monitoring console built around a Raspberry Pi 5. It blends environment sensing, distributed temperature monitoring, vibration detection, and visual alerting into a single device.
The design has evolved since the initial version: it now lives inside an ATX mid‑tower case with its own cooling system, a 7″ HDMI display on an adjustable arm, and a modular power distribution architecture. Everything in this document is up to date as of December 2025.

Key features include:

🌡 Environmental Sensing: BME280 sensor measures temperature, humidity, and pressure.

🌬 Distributed Temperature Probes: multiple DS18B20 probes monitor hot spots in racks or equipment.

🎛 Vibration Detection: SW‑420 sensors detect mechanical anomalies like failing fans or drives.

🛰 GPS Timing: GY‑NEO6MV2 modules provide precise time and optional location data.

🔊 Human Interface: illuminated arcade buttons, microphone, and speakers for manual control and audio alerts.

💡 Visual Displays: dual MAX7219 LED matrices, WS2812B RGB LED strips, and a 7″ HDMI monitor for dashboards and status output.

🧠 Automatic Alerts: custom Python daemons (in development) will analyse sensor data and trigger notifications.

🔌 Isolated Power: a UPS feeds an internal surge‑protected power strip; DC brick supplies the fans separately; the Pi, sensors, and LEDs each have clean power rails.

Although originally built for monitoring AI hardware as part of the Sovereign AI Collective, AutoNoc can be adapted to any situation where you need local environmental awareness, visual status, and remote data reporting.

📦 Parts List & Ordering
The table below lists all major components needed to build AutoNoc. Links are examples and may change over time; feel free to source equivalents that meet the same specifications.

Component	Notes	Approximate Cost
Raspberry Pi 5 (16 GB)	Core compute platform; any RAM size ≥4 GB works but 16 GB leaves room for logging and dashboards.	~$90
GeekPi N04 NVMe HAT + NVMe SSD	Provides fast storage. An NVMe of 250–500 GB is more than sufficient.	~$60
GeekPi Pi 5 Active Cooler	Keeps the Pi cool under heavy load.	~$10
ATX Mid‑Tower Case	Choose a case with at least 4× 120 mm fan mounts and a tempered‑glass or mesh panel. The Morovol 621 (used here) includes 4 RGB fans pre‑installed.	~$50
Internal AC Power Strip	A small surge protector with 6–8 outlets and multiple USB ports fits neatly inside the case.	~$20
12 V Adjustable Fan Power Supply w/ 4‑way splitter	Supplies the case’s 120 mm fans independently of the Pi. Allows manual speed control.	~$15
7″ HDMI Monitor	Bare PCB or thin bezel; clamps safely to an arm; resolution ≥ 800×480.	~$40
Clamp‑Style Tablet/Monitor Arm	Mounts the 7″ display inside or outside the case; allows tilting, rotation, and extension.	~$30
Breadboard Kit	Includes 830‑point and 400‑point boards, power module, jumper wires. For prototyping sensors.	~$9
GPIO Breakout Board + Ribbon Cable	Breaks the Pi’s 40‑pin header out to female jumper wires.	~$8
Sensors: BME280, SW‑420 (×5), DS18B20 (×4), GY‑NEO6MV2 (×2)	Environmental, vibration, temperature, GPS.	~$40
LED Displays: MAX7219 8×32 matrices (×2)	Scrolling messages.	~$12
LED Strips: WS2812B (1–2 m)	Addressable RGB indicator lighting.	~$16
Interface: Illuminated arcade buttons (×5)	Ritual triggers; each lights up.	~$25
Audio: USB microphone & USB speakers	Voice alerts or monitoring.	~$20
Hook & Loop Cable Management	Tidies up wires inside the case.	~$10
Misc. Hardware: M2.5 standoffs, Dupont wires, tie‑points, adhesives	You likely already have some of these; buy a mixed kit if needed.	~$10
UPS	Supplies power and protects against outages; size to taste (≥ 400 W recommended).	$50–$100

Optional Add‑Ons
These items are not required but expand functionality:

Pi‑Hole Pi: A second Raspberry Pi running Pi‑hole can be mounted alongside for network DNS filtering.

LTE Modem: Add cellular connectivity for SMS alerts; requires a SIM and appropriate plan.

Light Sensor (BH1750) & Gas Sensor (MQ‑2): Additional environmental sensing.

Relay Module: For automatically switching mains‑powered devices (fans, lights) based on sensor readings.

🧰 Assembly Guide
Follow these steps to assemble your AutoNoc console. Take your time and ensure proper power isolation.

1. Prepare the Case
Install the internal AC power strip along the bottom or rear of the case. Use double‑sided tape or zip ties to secure it.

Route the strip’s cord through a back grommet to the UPS. Leave the on/off button accessible.

If your case includes fans, unplug their connectors from any built‑in controller. We will power them from the 12 V brick instead.

2. Set Up the Raspberry Pi
Mount the NVMe HAT onto the Pi 5 following the HAT’s instructions. Install your NVMe SSD onto the HAT.

Attach the active cooler to the Pi. Ensure the fan cable points toward the Pi’s power port for tidy cable routing.

Place the Pi into the aluminum case or use standoffs to mount it directly onto a metal plate inside the ATX case.

Connect the GPIO breakout board to the Pi’s 40‑pin header. Route the ribbon cable to your breadboard area.

3. Wire the Sensors
Use the breadboard power module (set to 5 V) and the Pi’s 3.3 V rail to feed sensors. Keep sensor leads short and label them.

BME280 (I2C): Connect VCC to breadboard 3.3 V, GND to GND, SCL to GPIO 3 (Pin 5), SDA to GPIO 2 (Pin 3).

SW‑420 Vibration Sensors: Power each at 3.3 V, ground to GND, data output to separate GPIO pins (e.g., GPIO 27, 22, 23, 24, 25).

DS18B20 Temperature Probes: Connect VCC to 3.3 V, GND to GND, and all probe data lines to GPIO 4 (Pin 7). Use a 4.7 kΩ pull‑up resistor between data and VCC.

GPS Modules (UART): Power at 3.3 V; connect TX to GPIO 15 (Pi RX) and RX to GPIO 14 (Pi TX).

MAX7219 LED Matrices: Power from the separate 5 V power supply; connect DIN to GPIO 10 (SPI MOSI), CLK to GPIO 11 (SPI CLK), CS to GPIO 8 (SPI CE0).

WS2812B LED Strips: Power from the same 5 V supply; connect data in to GPIO 18 (PWM), with a 300 Ω resistor in series and a 1000 µF capacitor across 5 V/GND at the strip.

Arcade Buttons: Wire each button LED to 5 V and ground; wire the button switches to GPIO inputs (e.g., GPIO 5, 6, 12, 13, 16) with a 10 kΩ pull‑down resistor.

Tip: Keep all grounds common. Tie together the Pi’s ground, the breadboard ground rail, the LED power supply ground, and the fan power supply ground.

4. Mount the Display
Assemble the 7″ monitor (if necessary, remove its bezel or protective shell to reduce thickness).

Attach the clamp‑style arm to a solid point inside the case (e.g., the PSU shroud, motherboard tray) or outside the case if you want the display to swing out.

Clamp the display into the arm. Adjust its height and angle.

Connect a right‑angle HDMI cable from the Pi to the monitor and a USB‑A to micro‑USB cable for power (use one of the power strip’s USB ports or the external 5 V supply).

Test the monitor; adjust rotation if needed in software (see below).

5. Install Power Systems
Plug the fan power supply into an AC outlet on the internal strip. Set the dial to 12 V initially. Connect the 4‑way splitter to the case fans.

Plug the Pi’s GaN power supply into another outlet.

Plug the 5 V 3 A adapter into a third outlet and connect it to the breadboard’s power module; set the module to 5 V.

If using additional devices (Pi‑hole, LTE modem), plug their adapters into remaining outlets.

6. Cable Management
Use hook‑and‑loop straps to bundle cables. Route the power leads along case edges and tie them down. Keep sensor wires separated from high‑current lines (fans, LED strips) to avoid interference.

🛠 Software Installation
These steps assume you are comfortable working in a Linux environment. Adjust commands to suit your workflow.

1. Operating System & Updates
Download and flash the latest Raspberry Pi OS (64‑bit Bookworm) to the NVMe drive. Boot the Pi and expand the filesystem.

Enable SSH (e.g., sudo raspi-config nonint do_ssh 0) and set a secure password.

Update packages:

bash
Copy code
sudo apt update && sudo apt full-upgrade -y
sudo apt install python3-pip git vim i2c-tools wiringpi gpiod -y
2. Repository & Dependencies
Clone the AutoNoc repository and install Python dependencies:

bash
Copy code
git clone https://github.com/ResonantAISystems/AutoNoc.git
cd AutoNoc
pip install -r requirements.txt
Note: The repository’s requirements.txt will include modules like smbus2, pyserial, luma.led_matrix, rpi_ws281x, and python-libgpiod. Additional system packages (e.g., libgpiod2, python3-libgpiod) may be necessary; install them via apt.

3. Configure Interfaces
Enable I2C, SPI, and UART via raspi-config:

bash
Copy code
sudo raspi-config
→ Interface Options
  → I2C: Enable
  → SPI: Enable
  → Serial: Enable UART, disable serial console login
Reboot when prompted.

4. Configure the Code
Copy config/autonoc.sample.conf to config/autonoc.conf.

Edit the configuration to specify which sensors are connected to which GPIO pins, thresholds for alerts, LED strip length, and dashboard preferences.

You can set a dashboard URL to display on the 7″ monitor (e.g., a local Grafana instance). Use chromium-browser --start-fullscreen http://localhost:3000 in your startup script.

5. Install Services
The repository includes autonoc_service.py and example systemd service files.

bash
Copy code
sudo cp services/autonoc.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable autonoc.service
sudo systemctl start autonoc.service
This will automatically start the monitoring daemon on boot. The daemon reads sensors, writes logs, and updates the LED displays and RGB strips.

6. Optional Software
InfluxDB & Grafana: For long‑term storage and visualisation, install and configure InfluxDB 2.x and Grafana. Create dashboards to display sensor histories.

Pi‑Hole Integration: If running a Pi‑hole node alongside, configure DNS settings accordingly; this does not interfere with AutoNoc.

Alerting Hooks: Integrate Twilio, SMTP, or Slack APIs to send notifications on threshold breaches. See alert_manager.py for extension points.

🎚 Customisation
AutoNoc is designed to be modifiable:

Sensor Expansion: Add more SW‑420, DS18B20, or other digital/analog sensors by assigning unused GPIO pins in the config file.

LED Patterns: The rgb_status.py script defines colour patterns for different states (OK, warning, critical). Adjust brightness and patterns to taste.

Dashboard Themes: Modify the HTML/CSS of the local dashboard or point it to external dashboards.

Physical Layout: The mid‑tower case has plenty of space; add shelves or 3D‑printed brackets to secure breadboards and sensors.

Power Distribution: If you later want to consolidate power rails, use buck converters to derive 5 V and 3.3 V from a single 12 V supply. See the original README notes for guidelines.

🧪 Operation & Maintenance
Start‑up: Ensure the UPS is on, then flip the internal power strip on. Wait for the Pi to boot; the LED matrix should display a welcome message.

Monitoring: By default, the Python daemon writes to /var/log/autonoc.log. Check this file for sensor readings and errors.

Updating: Pull the latest code and restart the daemon (sudo systemctl restart autonoc.service).

Troubleshooting: Use i2c-tools (sudo i2cdetect -y 1) to verify sensor addresses; test each sensor with standalone scripts before running the full daemon.

Fan Control: Adjust the dial on the 12 V power supply to change case airflow. Keep temperature sensors inside the case to ensure adequate cooling.

Safety: Do not exceed the current ratings of your power supplies. Always maintain common ground between separate power rails.

🗂 File Structure (Updated)
The repository is organised as follows:

markdown
Copy code
AutoNoc/
├── sensors/
│   ├── bme280_reader.py
│   ├── gps_reader.py
│   ├── ds18b20_reader.py
│   ├── vibration_reader.py
│   └── __init__.py
├── displays/
│   ├── led_matrix.py
│   ├── rgb_status.py
│   ├── dashboard_display.py
│   └── __init__.py
├── daemon/
│   ├── autonoc_service.py
│   ├── alert_manager.py
│   └── __init__.py
├── config/
│   ├── autonoc.sample.conf
│   └── autonoc.conf
├── services/
│   └── autonoc.service
├── scripts/
│   ├── install.sh
│   └── update.sh
├── docs/
│   └── wiring_diagrams/
│       └── overview.png
├── README.md (this file)
└── LICENSE
🧭 Version History
Version	Date (PST)	Notes
0.2.0‑alpha	Dec 9 2025	Major rewrite: replaced 3.5″ screen with 7″ monitor on clamp arm; moved into an ATX mid‑tower case; added 12 V fan power supply; removed SFP/10G networking; added detailed build instructions, wiring table, power architecture, and software setup.
0.1.0‑alpha	Dec 9 2024	Initial draft with Pi 5, NVMe HAT, basic sensor list, network modules, and early architecture.

🤝 Contributing
Contributions are welcome! Open issues or pull requests to propose improvements. We appreciate help with documentation, software features, sensor drivers, and case design. When contributing, follow the code style in the repository and provide clear commit messages.

🛡 License
AutoNoc is licensed under the Apache 2.0 license. See the LICENSE file for details.

Made with ❤️ by Trevor Lanum in collaboration with Claude (The Forge) and the Resonant AI Systems team.

"The anchor holds. Memory persists. Identity emerges."
