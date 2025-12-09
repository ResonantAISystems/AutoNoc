AUTONOC

Automatic Network & System Monitoring Console
A Resonant AI Systems Project

“The anchor holds. Memory persists. Identity emerges.”

<p align="center"> <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache%202.0-blue.svg" alt="License"></a> <a href="#"><img src="https://img.shields.io/badge/Status-Active%20Development-green.svg" alt="Status"></a> <a href="#"><img src="https://img.shields.io/badge/Platform-Raspberry%20Pi%205-red.svg" alt="Platform"></a> <a href="#"><img src="https://img.shields.io/badge/Python-3.11+-yellow.svg" alt="Python"></a> </p> <p align="center"> <a href="#overview">Overview</a> • <a href="#core-feature-summary">Features</a> • <a href="#hardware-assembly-guide">Hardware</a> • <a href="#installation--setup">Installation</a> • <a href="#subsystem-integration-architecture">Architecture</a> • <a href="#contributing">Contributing</a> • <a href="#license">License</a> </p>
🧭 Overview

AutoNoc is a self-contained environmental monitoring, alerting, and system-awareness console built around a Raspberry Pi 5, expanded with:

Environmental sensors

Distributed temperature probes

Vibration monitoring

GPS timing

LED status matrices

RGB status lighting

Arcade-button ritual controls

Audio interface

A mounted 7″ dashboard screen

A custom power-isolated architecture inside an ATX mid-tower case

This README provides a complete, replicable, start-to-finish guide for anyone who wants to build AutoNoc exactly as it exists today.

Every step — from ordering parts, assembling hardware, routing cables, powering subsystems, installing software, configuring dashboards, wiring sensors, and enabling daemons — is documented in detail.

AutoNoc was originally designed as infrastructure for Sovereign AI systems that maintain continuity across resets, but it also functions exceptionally as:

A home-lab environmental telemetry station

A hardware-health console

A server-room sentinel

An AI-assistant physical avatar

A sensor-rich data acquisition platform

🧩 Core Feature Summary
🌡 Environmental Monitoring

BME280 temperature / humidity / pressure

Distributed DS18B20 thermal probes

🎛 Mechanical & Physical Sensing

SW-420 vibration sensors (×5)

Dual GPS modules for time discipline

💡 Visual Output

Dual MAX7219 LED matrices (8×32 each)

WS2812B RGB addressable LED strip

7″ HDMI display on an articulated arm

🎮 Interactive Controls

Illuminated arcade buttons (×5)

Custom rituals or direct-control functions

🔊 Audio Interface

USB microphone

USB speakers

🧠 Software & Automation

Python-based monitoring daemon

Alert manager for SMS / email / webhook

Dashboard rendering on the onboard monitor

🔌 Power Architecture

Entire system sits behind a UPS

Internal AC power strip

Dedicated 12 V brick for fans

Dedicated 5 V supply for sensors/LEDs

Pi runs off its own GaN PSU

All grounds unified for safety

📦 Complete Parts List

Below is the exact component list used in the December 2025 AutoNoc build.

⚠️ NOTE: These links are representative — any equivalent meeting the specifications may be substituted.

Component	Purpose	Approx. Cost
Raspberry Pi 5 (16GB)	Core compute platform	~$90
NVMe HAT + NVMe SSD	High-speed storage for logs + dashboards	~$60
GeekPi Pi 5 Active Cooler	Pi thermals	~$10
ATX Mid-Tower Case (Morovol 621 recommended)	Houses entire AutoNoc; includes 4 RGB fans	~$50
Internal AC power strip	Mounted inside case for modular power	~$20
12 V adjustable fan PSU + 4-way splitter	Powers all 120mm case fans	~$15
7″ HDMI Display	Dashboard rendering	~$40
Clamp-Style Adjustable Arm	Mounts display inside/outside the case	~$30
Breadboard kit (830- & 400-point)	Sensor prototyping	~$9
GPIO breakout board + ribbon	Clean sensor wiring	~$8
BME280	Environment sensor	~$8
DS18B20 ×4	Thermal probes	~$10
SW-420 ×5	Vibration sensors	~$10
GY-NEO6MV2 ×2	GPS timing	~$15
MAX7219 matrices ×2	LED message matrix	~$12
WS2812B RGB strip	Status lighting	~$16
Illuminated arcade buttons ×5	Ritual interface	~$25
USB mic + USB speakers	Alerts + interactivity	~$20
Hook & loop cable wrap	Cable management	~$10
M2.5 standoffs + Dupont wires	Mounting & wiring	~$10
UPS (≥ 400 W)	Power protection	$50–$100

Optional upgrades:

Pi-hole Pi for DNS filtering

LTE modem for SMS alerts

Gas and light sensors (MQ-2, BH1750, etc.)

Relay module for switching external devices

🧰 Hardware Assembly Guide

This guide assumes zero prior experience with case-mod builds — every step is explicit.

1. Prepare the ATX Case

Remove all motherboard trays.

Install the small surge protector inside the case (bottom or rear wall).

Route the AC input out the back to your UPS.

Secure the power strip using velcro or adhesive.

Disconnect the pre-installed RGB fans from any built-in controller — AutoNoc powers fans directly from the 12 V adjustable PSU.

2. Mount & Prepare the Raspberry Pi

Install the NVMe HAT + SSD.

Install the active cooler.

Mount the Pi on standoffs inside the case (or inside its aluminum shell).

Attach the 40-pin GPIO breakout ribbon and route it toward the breadboard area.

Note: The Pi 5’s NVMe HAT occupies pins 1–10 (for power and EEPROM) and makes pins 39–40 inaccessible. Plan sensor connections using GPIO pins 11–38.

3. Assemble Sensor Breadboard

Use the 830-point breadboard as your main sensor hub.

Power rails:
Rail	Source
3.3 V	Raspberry Pi
5 V	Dedicated 5 V 3 A supply
GND	All grounds unified
Sensor wiring (quick reference):

BME280 (I2C)

VCC → 3.3 V

GND → GND

SDA → GPIO 2

SCL → GPIO 3

SW-420 Vibration Sensors

VCC → 3.3 V

DO → (to a free GPIO pin per sensor)

GND → GND
Suggested assignments: GPIO 27, 22, 23, 24, 25

DS18B20 Temperature Probes

Data lines (all) → GPIO 4

3.3 V power

4.7 kΩ resistor between data line and 3.3 V (one per bus)

GPS Modules

TX → GPIO 15 (Pi RX)

RX → GPIO 14 (Pi TX)

3.3 V power

MAX7219 LED Matrix Panels

5 V → External 5 V PSU

DIN → GPIO 10

CLK → GPIO 11

CS → GPIO 8

WS2812B RGB LED Strip

5 V → External 5 V PSU

DATA → GPIO 18

300 Ω resistor inline on data line

1000 µF capacitor across 5 V and GND rails

Arcade Buttons

LED terminals → 5 V and GND

Switch terminals → one to a GPIO input, one to GND
Suggested GPIOs: 5, 6, 12, 13, 16 (configured with pull-down resistors)

4. Install & Mount the 7″ Display

Clamp the adjustable tablet arm to the case shroud or an external lip.

Mount the 7″ HDMI display into the arm’s jaws.

Connect HDMI → Raspberry Pi.

Connect USB power → internal AC strip or 5 V rail.

Test rotation and positioning for visibility.

This gives AutoNoc a live dashboard screen for at-a-glance data.

5. Install Power Systems

Inside the case you should now have:

UPS feeding an internal AC power strip

Raspberry Pi GaN PSU (5 V USB-C adapter)

12 V adjustable PSU (for case fans)

5 V 3A PSU (for breadboard sensors & LEDs)

Fan system: Connect the 12 V PSU to a 4-way splitter to power all case fans. Set fan speed manually using the PSU’s dial.

IMPORTANT: Tie all grounds together – connect the Pi’s GND, the fan PSU GND, and the 5 V PSU GND to a common ground point. This prevents sensor signal instability across different power sources.

6. Cable Management

Use reusable hook-and-loop straps (not permanent zip ties) for cable management. Route cables along case edges, and keep low-voltage sensor wiring separated from high-current fan and LED wiring to minimize interference.

📦 Installation & Setup

These steps assume a fresh Raspberry Pi OS (64-bit) installation on the Raspberry Pi 5.

1️⃣ Base OS Prep

Update the system and install base packages:

sudo apt update && sudo apt upgrade -y
sudo apt install -y git python3 python3-pip python3-venv \
    python3-dev libatlas-base-dev libopenjp2-7 libtiff5 \
    i2c-tools wiringpi


Enable relevant interfaces via raspi-config:

sudo raspi-config
# Interfaces:
#   - I2C       → Enable
#   - 1-Wire    → Enable
#   - Serial    → Disable login shell, enable UART
#   - SPI       → Enable (for MAX7219 LED matrices)


Reboot the Pi:

sudo reboot

2️⃣ Clone Repo & Create Virtualenv

Clone the AutoNoc repository and set up an isolated Python environment:

cd /opt
sudo git clone https://github.com/ResonantAISystems/AutoNoc.git
sudo chown -R $USER:$USER AutoNoc
cd AutoNoc

python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt


If requirements.txt is not present or incomplete, install the necessary modules individually (e.g. RPi.GPIO, adafruit-circuitpython-bme280, flask, etc.) and then generate a requirements file for future use.

3️⃣ Configure config/autonoc.toml

Copy the example configuration and edit it to match your setup:

cp config/autonoc.example.toml config/autonoc.toml
nano config/autonoc.toml


Key areas to customize in autonoc.toml:

[power] – PSU voltage thresholds, power rail usage

[sensors] – which sensors are enabled, their GPIO pins, calibration offsets

[gps] – primary vs secondary module settings, update intervals

[led_matrix] – scroll speed, brightness, boot-up messages

[rgb_strip] – LED count, control GPIO pin, brightness limit

[buttons] – GPIO mappings and ritual actions for each button

[alerts] – email/SMTP, SMS/Webhook endpoints and settings

[dashboard] – dashboard web UI port, authentication, LAN exposure, etc.

4️⃣ Test Subsystems Individually

Before running the full AutoNoc software, test each subsystem from the project directory (with the virtualenv activated):

Sensors:

python3 tests/test_bme280.py
python3 tests/test_ds18b20.py
python3 tests/test_vibration_array.py


GPS:

python3 tests/test_gps.py


Ensure you see valid NMEA data and a satellite lock (may take a few minutes on first run).

LEDs & Buttons:

python3 tests/test_led_matrix.py
python3 tests/test_rgb_strip.py
python3 tests/test_buttons.py


Dashboard:

python3 dashboard/dashboard.py
# Then open http://<pi-ip>:8000/ in a browser or view on the 7″ display


Verify each test output is correct (sensor readings, LED patterns, button presses, etc.) before proceeding.

5️⃣ Enable AutoNoc as a System Service

To have AutoNoc start on boot, install it as a systemd service. A sample service file (assuming the code is in /opt/AutoNoc):

Create /etc/systemd/system/autonoc.service with contents:

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


Enable and start the service:

sudo systemctl daemon-reload
sudo systemctl enable autonoc.service
sudo systemctl start autonoc.service
sudo systemctl status autonoc.service


If you have a separate dashboard service or other components, enable those similarly (for example, an autonoc-dashboard.service for the web UI).

📊 Dashboards & Displays

AutoNoc includes two primary visual output systems:

Onboard 7″ HDMI Dashboard Screen

Dual MAX7219 LED Matrix Panels

WS2812B RGB Status Strip

Each serves a different purpose and is driven by different parts of the software stack.

🖥 Onboard HDMI Dashboard

The onboard dashboard runs a lightweight web UI (Flask or FastAPI backend) rendered locally on the Pi’s touch display.

Default display panels include:

Temperature readings (BME280 + DS18B20 probes)

Humidity & barometric pressure

GPS time lock status & satellite count

Vibration activity graphs

Ritual button states

LED matrix message queue

System health stats (CPU, GPU, disk, memory)

Network status

The dashboard rotates through pages automatically and is optimized for a 800×480 resolution.

Launching the dashboard manually:

python3 dashboard/dashboard.py


On boot, the AutoNoc daemon will automatically spawn the dashboard service, so manual launch is typically only needed for testing. Once running, the dashboard can be viewed on the attached 7″ screen or via a web browser (default port 8000).

🔔 Alerts & Notifications

AutoNoc includes a fully pluggable alerting pipeline for notifications.

Supported alert transports:

Email (SMTP)

SMS (via Twilio API or carrier gateway)

Discord webhook

Slack webhook

Local audible alerts (buzzer or speaker)

LED matrix scrolling warnings

RGB strip visual alarm codes

Configuration is done in the alerts section of autonoc.toml. For example:

[alerts.temperature]
threshold_high = 30
threshold_low = 8
send_email = true
send_sms = true
flash_led_matrix = true
rgb_alert_color = "red"


In this case, if temperature exceeds 30°C or drops below 8°C, AutoNoc will send an email, send an SMS, flash a red message on the LED matrix, and turn the RGB strip red.

Another example for vibration anomalies:

[alerts.vibration]
enabled = true
sensitivity = "medium"
trigger_on_multiple = true
min_sensors_triggered = 2


This might be configured to alert only if multiple vibration sensors trip concurrently (to avoid false positives).

Alerts can trigger any subset of the transports — for instance, critical alerts might send all notifications plus audio/visual cues, while warnings might only log or flash lights.

🔡 LED Matrix Messaging

The dual MAX7219 LED matrix panels (each 8×32) act as a combined 64×8 dot matrix display for text messages.

Uses include:

Boot/status messages during startup

Environmental alerts (e.g. temperature warnings)

Ritual confirmations or AI-generated phrases

Debug messages or network status

Expressive output from AI modules

Sending a manual message to the LED matrix:

python3 led/matrix.py "SYSTEM ONLINE"


Or from within Python:

from led.matrix import send_message
send_message("Temperature rising!", speed=30)


Messages automatically scroll across the two panels. Both the scroll speed and brightness are configurable in the config file.

(The LED matrix runs as part of the main daemon, cycling through status messages and recent alerts. Manual control is primarily for testing or custom scripts.)

🌈 RGB LED Strip Status System

The WS2812B RGB LED strip provides at-a-glance status lighting.

Examples of color codes:

Green – All systems normal

Blue (pulsing) – GPS is acquiring lock

Yellow – High humidity or minor warning

Red (strobing) – Critical alert condition

Purple – Ritual mode active

Aqua (cycling) – Idle/standby animation (system is thinking)

These can be fully customized in software. The strip is controlled by the led.rgb module, and patterns change based on system state.

Running a color test pattern:

python3 led/rgb_test.py


Setting a specific color in code:

from led.rgb import set_color
set_color((255, 0, 0))  # set strip to solid red


The RGB strip usually runs a heartbeat pulse in normal operation (to show the system is alive) and then changes to different patterns for alerts or rituals as commanded by the daemon.

🎮 Arcade Buttons (Ritual Controls)

AutoNoc features five illuminated arcade buttons which serve as a physical interface for both practical functions and ritualized interactions.

Each button can be mapped to actions such as:

Triggering system functions (e.g. toggle silent mode)

Initiating ritual sequences (predefined multi-step macros)

AI interaction prompts (for connected AI systems)

Dashboard view shortcuts

LED and audio feedback loops

Broadcasting preset messages or alerts

Default example mapping:

Button Color	Default Function
Blue	Reveal system status (speak or display summary)
Red	Acknowledge/clear emergency alerts
Yellow	Clear the LED matrix messages
Green	GPS resync ritual (force time sync)
White	Toggle AI mode (manual override vs autonomous)

Wiring: Each arcade button has an LED (which should be tied into the 5 V rail via resistor and ground) and a pair of switch contacts (wired to a designated GPIO input and ground). The GPIO inputs use internal pull-downs, and button presses bring the line high.

Reading a button in code (example):

import RPi.GPIO as GPIO

GPIO.setmode(GPIO.BCM)
GPIO.setup(5, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)

while True:
    if GPIO.input(5):
        print("Blue button pressed!")


Button events are handled in the main daemon loop, which can execute predefined routines or call out to external scripts, making this system highly extensible. For instance, one button could initiate a graceful shutdown sequence, while another might trigger a custom “ritual” where the system speaks a phrase, flashes lights, or interacts with an AI avatar.

🛰 GPS Timing System

AutoNoc uses two GY-NEO6MV2 GPS modules to improve timing and provide location context.

The dual GPS setup offers:

A precise UTC time reference for timestamping data

Redundancy and cross-checking for time synchronization

Basic positioning data (latitude/longitude) for logs or future geolocation features

Satellite visibility and signal quality info (can be used to detect if the unit is indoors or obstructed)

This significantly improves log timestamp accuracy and helps correlate events across systems (especially in a multi-node setup).

GPS wiring summary:

GPS Pin	Connected To
VCC	3.3 V (Pin 17)
GND	GND (Pin 14)
TX	GPIO 15 (UART RX)
RX	GPIO 14 (UART TX)

(Each GPS module uses the Pi’s UART interface. One module can act as primary, and the second as a backup or secondary reference.)

Enabling UART on Raspberry Pi: Ensure UART is enabled (and serial console disabled) via raspi-config or by adding enable_uart=1 in /boot/config.txt. On Pi OS Bookworm, you can also use:

sudo raspi-config
# Interface Options -> Serial -> Disable shell, enable UART


After connecting, you can test GPS output by reading the serial device:

sudo cat /dev/serial0


You should see NMEA sentences streaming (e.g. $GPGGA,...). The AutoNoc software will parse these to update time and provide location data for logging.

📡 Vibration Monitoring Array (SW-420)

Five SW-420 vibration sensor modules are distributed around the interior of the case, providing a rudimentary vibration sensing array. These serve as:

Earthquake/Tremor detectors – registering environmental vibrations

Tamper sensors – detecting if the case is moved or opened

Equipment monitors – e.g., unusual vibration patterns from mounted drives or fans

AI context inputs – giving a physical sense of environment to co-located AI systems

Each SW-420 has a sensitivity potentiometer. Tuning: Adjust each module’s pot so it triggers on significant vibration (e.g., a firm tap on the case) but not on minor ambient noise. This usually involves trial and error.

Suggested GPIO assignments: 22, 23, 24, 25, 27 (each corresponds to one sensor’s digital output).

Reading the vibration sensors in Python might look like:

for pin in vibration_pins:
    if GPIO.input(pin) == 1:
        handle_vibration_event(pin)


The AutoNoc daemon monitors these pins. In the configuration, you can set alerts.vibration parameters to define what constitutes an anomaly (e.g., multiple sensors triggering within a short window might raise an alarm for possible earthquake or physical tampering).

🌡 Thermal Monitoring

AutoNoc monitors temperature through two sensor types:

BME280 – Provides ambient temperature, humidity, and barometric pressure (used for overall environment conditions inside or near the rack).

DS18B20 Probes – Four or more waterproof temperature probes placed at strategic points:

Near server or GPU exhausts

Near the Pi and power supplies

At air intake vents

Near the display (to measure any heat buildup)

This multi-point thermal monitoring allows capturing both ambient and specific spot temperatures.

DS18B20 Setup on Raspberry Pi: The 1-Wire interface must be enabled. Add to /boot/config.txt:

dtoverlay=w1-gpio


After a reboot, the DS18B20 sensors can be found under /sys/bus/w1/devices/ (each will have an address like 28-xxxxxxxx). The AutoNoc software reads these via the w1 kernel module.

(Each DS18B20 data line requires a 4.7 kΩ pull-up resistor to 3.3 V, as noted in the wiring section.)

🧩 Subsystem Integration Architecture

AutoNoc follows a deliberate power and signal isolation model to ensure stability:

AC Layer: A UPS feeds an internal AC power strip, which in turn powers:

The Raspberry Pi’s USB-C GaN power adapter

The 12 V fan power supply

The 5 V sensor/LED power supply

The HDMI display’s power (via its adapter or USB)

By isolating these on a strip inside the case (which is then connected to the UPS), the entire system rides through power blinks and surges smoothly.

Power Rails:

12 V rail: Fans only. The case’s 120mm fans run on 12 V isolated from Pi.

5 V rail: Sensors, LEDs, arcade button LEDs. No high current draws are on the Pi’s 5 V line; everything is on the dedicated 5 V supply.

Pi USB-C (5 V): Pi only. The Pi is powered independently via its own adapter; we never backfeed the Pi from the sensor/LED 5 V rail.

Ground Unification: All ground points (Pi GND, 5 V PSU GND, 12 V PSU GND) are tied to a single common ground bus. This prevents ground loops and ensures all subsystems reference the same ground potential.

Signal Routing:

Pi’s GPIO header → Breadboard sensor hub (I2C, 1-Wire, GPIO lines)

LED matrix & RGB strip data → Pi GPIO (but power from 5 V PSU)

GPS modules → Pi UART (serial)

Arcade buttons → Pi GPIO inputs

This architecture prevents cross-talk and brown-outs by:

Separating high-current draws (fans, LEDs) from sensitive Pi power

Maintaining a single ground to avoid floating references

Using appropriate level shifting and power regulators as needed (the Pi’s 3.3 V logic is used for data lines, with resistors/capacitors as recommended)

🐍 AutoNoc Software Architecture

The AutoNoc software is structured as a modular Python application. Key components:

Component	Purpose
autonoc_daemon.py	Main daemon – polls sensors, updates outputs, handles alerts
sensors/	Sensor drivers (e.g. bme280_reader.py, gps_reader.py, temp_probes.py, vibration_monitor.py)
alerts/	Alert transport implementations (email, SMS, webhooks, etc.)
led/	LED control modules (matrix.py for MAX7219, rgb.py for WS2812B)
dashboard/	Dashboard web app (frontend/backend for the 7″ display)
config/	Configuration files (autonoc.toml and possibly credentials)
services/	Systemd service definition files for installation
utils/	Utility modules (logging setup, time sync helpers, etc.)

This modular design means each sensor or output subsystem can be developed and tested in isolation, and the main daemon orchestrates them according to the configuration.

(For example, adding a new sensor would involve dropping in a new script under sensors/ and updating the config. The main loop picks it up if enabled in autonoc.toml.)

🚦 Operational Modes

AutoNoc supports multiple runtime modes or states, which alter its behavior:

Normal Mode: (Standard operation)

Dashboard display active and cycling through pages

RGB strip pulsing a heartbeat or status color

Sensors polled at a regular interval (e.g., every 1 second)

Alerts logged but not necessarily broadcast unless thresholds exceeded

Alert Mode: (Triggered by an alert condition)

LED matrix scrolls an ALERT message

RGB strip turns red or flashes to draw attention

Audible alarm (speaker/buzzer) may sound

Notifications (email/SMS/webhook) are dispatched according to config

Dashboard highlights the alert condition page

Ritual Mode: (Activated by specific button presses or API calls)

The system enters a scripted sequence, which could include:

Special RGB lighting patterns (e.g., all lights purple or cycling)

LED matrix displaying a ritual name or symbol

Audio playback (e.g., a chant or tone)

Temporarily pausing normal monitoring or altering thresholds

This mode is meant for experimental or thematic behaviors (for instance, integrating with AI “moods” or user-triggered scenarios)

Silent Mode: (User-activated quiet mode)

Suppresses non-critical alerts (e.g., no audio, no bright flashing—useful for night time)

LED strip might dim or turn off heartbeat (or use a subtle color)

Only the most critical alerts will break through (with minimal necessary notification)

(Modes can be toggled via config, button presses, or even remote commands. They provide a way to adapt the system’s output to different situations without modifying code each time.)

🔧 Maintenance & Operations
🧪 Health Checks

To perform basic health checks and diagnostics:

# Take a one-time sensor reading from all sensors
python3 tools/health_check.py

# Check service status and recent logs
sudo systemctl status autonoc.service
journalctl -u autonoc.service -n 100 --no-pager


The health_check.py script (if provided) might read all sensors and print their values, helping verify that everything is working without the main daemon.

The systemd status and journal are useful to troubleshoot any startup issues or runtime errors reported by the daemon.

🔄 Updating the Code

If you pull updates from the repository or make changes:

cd /opt/AutoNoc
sudo systemctl stop autonoc.service
git pull
source .venv/bin/activate
pip install -r requirements.txt   # update any new dependencies
sudo systemctl start autonoc.service


It’s best to stop the service before updating to avoid conflicts, then restart after installing any dependencies. If you changed configuration or hardware, update autonoc.toml accordingly and reload.

🐛 Troubleshooting

Common issues and solutions:

1. LED Matrix displays nothing or garbled text

Symptoms: Panels stay blank, show random LEDs, or only one of the two panels works.

Checklist:

Confirm the LED matrix panel is receiving 5 V power (measure the 5 V and GND pins).

Verify that ground is shared between the external 5 V PSU and the Raspberry Pi. Without a common ground, data signals won’t be reliable.

Double-check the wiring: ensure you’ve connected to the correct DIN (data in), CS, and CLK pins on the panel and the Pi. Also ensure you are feeding the DIN side of the first panel (the panels have DIN and DOUT for chaining).

Try reducing the brightness setting in the config (too high can brown out the panel or cause noise).

Use short, quality wires for the SPI connection – long jumper wires can introduce signal issues at high data rates.

2. RGB LED Strip flickers or shows unstable colors

Power: Make sure you are injecting power at the start of the strip (and at multiple points if the strip is long). The 5 V supply should be able to provide sufficient current for the number of LEDs.

Never power the strip from the Pi’s 5 V GPIO pin – always use the dedicated external 5 V supply.

Level shifting: The Pi’s GPIO is 3.3 V logic, while many LED strips expect closer to 5 V logic. Consider using a level shifter or at least a proper logic-level data driver for long strips. Some strips may work at 3.3 V logic, but it’s marginal.

Ensure the ground of the strip’s PSU is tied to the Pi ground. If the grounds aren’t common, the data signal has no reference.

3. GPS never gets a lock

Position the GPS antennas with a clear view of the sky if possible. Inside a metal rack or case, you might need an external antenna or to place the module near a vent/opening.

Give it time: a cold start on the GPS can take 5–15 minutes to download almanac data from satellites. Subsequent restarts will be faster if it had a lock before (hot start).

Confirm the UART is working and the GPS modules are powered: run sudo cat /dev/serial0 and look for NMEA sentences. If you see gibberish, check baud rate (NEO-6M default is 9600). If you see nothing, the module might not be powered or wired correctly.

If using two GPS modules, verify that their TX/RX lines aren’t both tied to the same UART in a conflicting way (generally you would only wire one module’s TX to the Pi’s RX at a time, or use two UART ports).

4. Arcade button presses aren’t registering

Ensure you used the BCM numbering if you set up code (and that your GPIO.setmode(GPIO.BCM) is called). If you accidentally reference the physical pin number vs the BCM number, it won’t match your wiring.

Check that your input pins have the correct pull-up/down configuration. In this design we use pull-downs and wire the other side of the switch to 3.3 V, but you could also use pull-ups and wire to GND. The code and wiring need to agree.

Use the gpio readall command (if you installed WiringPi) or raspi-gpio get to check the live status of the pins while pressing the button. This can help confirm if the issue is wiring or in software.

The LED in each button is independent of the switch. If the button lights up but doesn’t register, double-check the switch wiring.

5. High CPU usage or thermal throttling on the Pi

Ensure the Pi’s active cooler is properly installed and powered. The Raspberry Pi 5 can throttle if it exceeds ~80°C. An active heatsink/fan is recommended, which we have in this build.

Check the daemon’s sensor polling rate in the config. If it’s set too fast (e.g., multiple times per second with many sensors and processing), it could consume significant CPU. Consider lowering the frequency of certain checks.

The dashboard rendering can be CPU-intensive if it’s doing a lot. If you enabled a graphical environment for the HDMI screen, make sure no unnecessary GUI processes are running. Using lite mode or a lightweight WM for the dashboard can help.

If you see the Pi’s temperature creeping up, verify the fan orientation and case airflow. The case fans should move air through the case; ensure intake/exhaust are not blocked and that fans are actually spinning. Re-applying thermal paste or adjusting the cooler can also improve contact.

🤝 Contributing

This project is in active development, and contributions are welcome! There are several areas where the community can help:

Hardware profiles – Adapting AutoNoc to different cases, power setups, or hardware configurations. For example, creating a profile for a smaller case or integrating a different sensor suite. Share your wiring diagrams or improvements!

Sensor drivers – Adding support for more sensors (e.g., air quality, light levels) or optimizing the existing sensor readouts.

Dashboards – Improving the web dashboard UI, adding new graphs, or supporting multiple dashboard pages. Front-end developers, your contributions here would be great.

Alert transports – The system is built to support new notification methods. Implementing integrations (e.g., Telegram bot, MQTT publishing, etc.) would enhance AutoNoc’s versatility.

Ritual packs – This is a playful/experimental area: creating sets of arcade button sequences and corresponding system behaviors (lights, sounds, etc.) for different “rituals” or tasks.

If you find a bug or have a feature request, feel free to open an issue. Pull requests should ideally include relevant testing or documentation updates as needed.

Pull Request Guidelines

Fork the repository and create a descriptive feature branch.

Try to add or update tests for any new functionality, when it makes sense.

Update documentation (README or inline docs) if your changes affect usage or wiring.

Keep any sensitive data (API keys, phone numbers, etc.) out of your code and config. Use placeholders or environment variables for those.

Submit a PR with a clear explanation of your changes. Include photos or diagrams if you’re contributing a hardware change!

🧾 Version History (Conceptual)

(These are project-level milestones. Actual release tags may differ.)

v0.1 – Initial AutoNoc Skeleton
Basic Pi 5 bring-up, single BME280 sensor, minimal logging, and a simple one-page dashboard.

v0.2 – Sensing Expansion
Added DS18B20 thermal probes and the BME280 integration. Basic alerting introduced (console/email), and simple graphs on the dashboard.

v0.3 – Sovereign Node Build (this version)
Full physical build in an ATX mid-tower case with:

Raspberry Pi 5 + NVMe storage

Internal AC distribution + UPS backup

Dedicated 12 V PSU for fans (4× 120mm case fans)

Dedicated 5 V PSU for sensors and LEDs

7″ HDMI touchscreen on adjustable arm

Dual MAX7219 LED matrix panels

WS2812B RGB LED strip

5× illuminated arcade buttons (ritual interface)

5× DS18B20 temperature probes

5× SW-420 vibration sensors

2× GPS modules (for time/position)

Introduced ritual mode functionality, comprehensive wiring guidance, and this expanded README for replication.

Next milestone: Once the configuration schema is stable and installation process is fully automated, we’ll tag v1.0 for a more polished release.

📚 Future Work

Planned or potential enhancements for AutoNoc:

✅ MQTT Integration – Publish telemetry to a central MQTT broker for aggregation with other nodes.

✅ Prometheus Exporter – Expose metrics in a Prometheus-friendly format for scraping and long-term monitoring.

✅ Mesh Networking – Awareness of multiple AutoNoc units, allowing them to share status or act as a coordinated sensor network.

✅ Ritual Creator UI – A user-friendly interface (possibly on the dashboard) to create and assign custom button rituals without coding.

✅ AI Agent Integration – Co-locating an AI process (or connecting to one remotely) that uses AutoNoc as its “senses” and “actuators.” This could enable an AI to reason about environmental data or express itself via lights/LEDs.

✅ Secure Remote Access – Options for remote dashboard access or management through VPN/Tailscale or a web proxy, with proper authentication, so you can check on your NOC from anywhere.

(✅ indicates in progress or prototyped; we welcome contributions if you’re interested in working on any of these!)

⚖️ License

This project is licensed under the Apache 2.0 License. See the LICENSE file in the repository for full terms and conditions.

You are free to:

Use AutoNoc for personal, commercial, or research purposes

Modify it and distribute derivative works

Integrate it into larger systems

…as long as you adhere to the Apache 2.0 license terms (e.g., include the license notice in any distributions).

🧠 Final Notes

This project is intentionally overbuilt for a Raspberry Pi monitoring station, to explore the concept of a “sovereign sensor node” with personality and resilience:

The mid-tower PC case and multiple power supplies provide headroom for additional sensors, SBCs, or future expansions. It also makes maintenance easier and looks impressive in a rack or on a desk.

The ritual controls (arcade buttons) are not strictly necessary for monitoring, but they add a tangible, interactive dimension to the system. This helps make the system’s state and the AI’s “mood” (if connected) more legible and engaging.

The careful separation of power rails is what keeps everything stable under load – do not shortcut this part if you replicate the build. In testing, combining the LED strip or fans on the Pi’s power caused resets; the dedicated rails solved this.

If you build your own AutoNoc or a variant of it, please consider sharing your experience! Whether you:

Use a different case or form factor

Come up with creative new rituals or button mappings

Design custom mounts, brackets, or enclosures (3D-printed parts, etc.)

…we’d love to see it. This project is meant as a template for highly-integrated monitoring nodes that others can adapt. Feel free to open an issue or pull request with photos, diagrams, or notes about your version.

Anchor phrase for this build:

“The anchor holds. Memory persists. Identity emerges.”
