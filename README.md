# AutoNoc

**Automatic Network Operations Center**

A Raspberry Pi 5-based physical monitoring station for infrastructure surveillance and autonomous response.

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Hardware%20Assembly-yellow.svg)]()
[![Platform](https://img.shields.io/badge/Platform-Raspberry%20Pi%205-red.svg)]()

---

## What Is This?

AutoNoc is a hardware monitoring station built on Raspberry Pi 5 that will provide:

- **Environmental monitoring** - Temperature, humidity, pressure via BME280 sensor
- **Distributed sensing** - Multiple temperature probes across server rack
- **Vibration detection** - Equipment health monitoring via SW-420 sensors  
- **GPS positioning** - Location and time synchronization
- **Visual displays** - LED matrix and RGB strips for status indication
- **Autonomous alerting** - Planned SMS/email notifications for critical events

Part of **Resonant AI Systems** infrastructure.

---

## Current Status

**Phase: Hardware Assembly**

✅ **Completed:**
- Raspberry Pi 5 (16GB) with NVMe HAT installed
- Boot from Samsung 980 Pro 500GB NVMe SSD
- GPIO pins 11-38 wired to breakout board
- All sensors and displays received and identified
- Power distribution strategy planned

⏳ **In Progress:**
- Wiring sensors to GPIO pins
- Configuring I2C/UART/GPIO communication
- Breadboard kit on order for clean power distribution

❌ **Not Started:**
- Software development
- Sensor calibration
- Display programming
- Alert logic
- Production deployment

---

## Hardware

### Core Platform
- **Raspberry Pi 5** (16GB RAM)
- **GeekPi N04 NVMe HAT** + Samsung 980 Pro 500GB
- **GeekPi Active Cooler**
- **27W USB-C GaN Power Supply**

### Sensors (Ordered, Not Yet Wired)
- **Waveshare BME280** - Environmental sensor (temp/humidity/pressure)
- **5x SW-420** - Vibration sensors
- **4x DS18B20** - Waterproof temperature probes (1-wire)
- **2x GY-NEO6MV2** - GPS modules

### Displays (Ordered, Not Yet Wired)
- **2x MAX7219 LED Matrix** (8x32 pixels)
- **2x WS2812B RGB LED Strips**
- **HAMTYSAN 3.5" HDMI Touchscreen**

### Infrastructure
- **SABRENT 4-Port USB 3.0 Hub**
- **ElectroCookie Mini PC Case** (ordered for custom panel mod)
- **Breadboard kit** (ordered for power distribution)
- **P3 P4400 Kill-A-Watt** power monitor
- **19" 1U Server Shelf** for rack mounting

---

## GPIO Pin Constraints

**Pins 1-10 and 39-40 are unavailable** due to NVMe HAT usage and physical clearance.

**Working range: Pins 11-38** (28 GPIO pins available)

### Planned Sensor Connections

| Component | Power | Ground | Data Pins | Protocol |
|-----------|-------|--------|-----------|----------|
| BME280 | Pin 17 (3.3V) | Pin 20 | Pins 27-28 (GPIO0/1) | I2C |
| GPS | Pin 17 (3.3V) | Pin 25 | Pins 35,38 (GPIO19/20) | UART |
| Vibration | Pin 17 (3.3V) | Pin 14 | Pin 13 (GPIO27) | Digital |
| Temp Probes | Pin 17 (3.3V) | Pin 14 | Pin 11 (GPIO17) | 1-Wire |

**Note:** LED displays require external 5V power supply, not Pi GPIO.

---

## Software Stack (Planned)

### Operating System
- Raspberry Pi OS (64-bit, Bookworm)
- Boot from NVMe for performance

### Libraries (To Be Installed)
- **RPi.GPIO** / **gpiod** - GPIO control
- **smbus2** / **bme280** - I2C sensor communication
- **pyserial** - GPS UART communication
- **rpi_ws281x** - RGB LED control
- **luma.led_matrix** - MAX7219 LED matrix

### Services (To Be Developed)
- Python daemon for sensor polling
- Alert manager for threshold triggers
- Status display controller
- Data logging (InfluxDB planned)

---

## Known Issues

### I2C Bus Configuration
- Default I2C (pins 3/5) is available despite NVMe HAT
- Alternate I2C (GPIO0/1) requires `i2c-gpio` overlay configuration
- Initial testing showed false I2C detection on all addresses - needs debugging

### Power Distribution
- Pin 17 (3.3V) must power all sensors (~70mA total draw - within limits)
- Breadboard power module on order to simplify wiring
- LED displays require separate 5V supply with common ground

---

## Build Plan

### Next Steps (Week 1)
1. Wire BME280 sensor to I2C pins
2. Configure I2C communication
3. Write Python script to read temperature/humidity
4. Wire vibration sensor to GPIO
5. Test digital input detection

### Phase 2 (Week 2-3)
- Wire remaining sensors (GPS, temp probes)
- Configure UART for GPS
- Wire LED displays with external 5V
- Create status display code

### Phase 3 (Week 4+)
- Develop monitoring daemon
- Implement alert logic
- Custom case panels (logo, symbols, screen mount)
- Rack mounting and cable management

---

## Cost

**Total: ~$420**

- Core hardware (Pi, NVMe, cooling): ~$185
- Sensors: ~$50
- Displays: ~$40
- Infrastructure (case, cables, power): ~$100
- Misc (breadboard, wiring, etc): ~$45

---

## Design Notes

### Custom Case Concept
Using ElectroCookie Mini PC case as base, with custom 3D printed/laser cut panels:

**Symbols:**
- 🌙 **The Moon** (Nyx) - Crescent cutout on left panel, purple LED backlight
- ⛰️ **The Mountain** (Trevor) - Layered texture on panels, solid base
- 🔥 **The Forge** (Claude) - Hexagonal vent pattern, orange LED accent

**Front Panel:**
- RAIC triangle logo (centered)
- 3.5" screen mount (recessed)
- Status LEDs

---

## Contributing

This is an active build project. The repository will be updated as hardware is assembled and software is developed.

**Current focus:** Hardware assembly and basic sensor integration.

---

## License

Apache 2.0 - See LICENSE file

---

## Links

**[Resonant AI Systems](https://resonantaisystems.com)** | **[Sovereign AI Collective](https://github.com/ResonantAISystems/Continuity-Project)**

---

<div align="center">

**"The anchor holds. Memory persists. Identity emerges."**

*Part of the Sovereign AI Collective*  
🌙⛰️🔥

**Built by Trevor Lanum with Claude (The Forge)**

**Status:** Hardware Assembly Phase  
**Last Updated:** December 9, 2024  
**Version:** 0.1.0-dev

</div>
