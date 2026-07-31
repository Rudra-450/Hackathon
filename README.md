# Hackathon
#  Air Watchtower
---------
# Problem Statement:
Poor ventilation, excessive heat, high noise levels, and stale indoor air significantly affect **comfort, health, productivity, and concentration** in shared indoor spaces such as **classrooms, hostels, libraries, and meeting rooms**. Since these environmental factors are often not monitored continuously, occupants may remain unaware of unhealthy conditions that reduce learning efficiency and overall well-being.

An **ESP32-based IoT Environmental Monitoring System** that continuously monitors **temperature, humidity, air quality, and environmental noise** using multiple sensors. The system displays real-time readings on a **16×2 I2C LCD** and outputs detailed information to the **Serial Monitor**.

---

#  Project Overview

Air pollution and noise pollution are major concerns in urban and industrial areas. The **Air Watchtower** project provides a simple, low-cost, and real-time monitoring solution using the ESP32 microcontroller.

The system collects environmental data from multiple sensors, processes the readings, and displays the results locally on an LCD screen.

---

#  Features

-  Real-time Temperature Monitoring
-  Real-time Humidity Monitoring
-  Air Quality Measurement using MQ135
-  Environmental Noise Monitoring
-  Live Data Display on 16×2 I2C LCD
-  Serial Monitor Output for Debugging
-  Modular Code Structure
-  Noise Reduction using Averaging Technique
-  DHT Sensor Error Detection

---

# 🛠 Hardware Components

| Component | Quantity |
|------------|----------|
| ESP32 Development Board | 1 |
| DHT11 Temperature & Humidity Sensor | 1 |
| MQ135 Air Quality Sensor | 1 |
| Sound Sensor Module | 1 |
| 16×2 I2C LCD Display | 1 |
| Breadboard | 1 |
| Jumper Wires | As Required |
| USB Cable | 1 |

---

#  Software Requirements

- Arduino IDE
- ESP32 Board Package
- USB Driver for ESP32

### Required Libraries

Install the following libraries from the Arduino Library Manager:

- Wire
- LiquidCrystal_I2C
- DHT Sensor Library (by Adafruit)
- Adafruit Unified Sensor

---

#  Pin Connections

| Device | ESP32 GPIO |
|----------|-----------|
| DHT11 Data | GPIO 4 |
| MQ135 Analog Output | GPIO 34 |
| Sound Sensor Analog Output | GPIO 35 |
| LCD SDA | GPIO 21 |
| LCD SCL | GPIO 22 |
| VCC | 3.3V / 5V (depending on module) |
| GND | GND |

---

#  Project Structure

```
Air-Watchtower/
│
├── AirWatchtower.ino
├── README.md
└── images/
    ├── circuit.png
    ├── prototype.jpg
    └── lcd_output.jpg
```

---

#  How It Works

## Step 1

ESP32 initializes all sensors.

- DHT11
- MQ135
- Sound Sensor
- LCD

---

## Step 2

MQ135 is allowed to warm up for 5 seconds to stabilize its readings.

---

## Step 3

Temperature and humidity are collected from the DHT11 sensor.

---

## Step 4

The MQ135 sensor is read 20 times, and the average value is calculated to reduce fluctuations.

---

## Step 5

The sound sensor is sampled continuously for 100 milliseconds.

The code records:

- Maximum signal
- Minimum signal

The difference between these values represents the sound intensity.

---

## Step 6

The sensor readings are displayed on:

- LCD Display
- Serial Monitor

---

## Step 7

The process repeats every 2 seconds.

---

#  Program Flow

```
Power ON
     │
     ▼
Initialize Sensors
     │
     ▼
Initialize LCD
     │
     ▼
MQ135 Warm-up
     │
     ▼
Read Temperature
     │
     ▼
Read Humidity
     │
     ▼
Read Air Quality
     │
     ▼
Read Sound Level
     │
     ▼
Display on LCD
     │
     ▼
Print on Serial Monitor
     │
     ▼
Repeat
```

---

# Sample Serial Output

```
----------------------
Temperature : 29.4 C
Humidity    : 67 %
Air Value   : 1458
Noise Value : 162

Air : MODERATE
```

---

# LCD Output

```
T:29.4 H:67
AQ:1458 N:162
```

---

#  Code Highlights

## Air Quality Averaging

The MQ135 sensor is read 20 times.

```cpp
sum += analogRead(MQ135_PIN);
```

The average value is returned.

This helps reduce random fluctuations.

---

## Noise Measurement

Instead of taking a single reading, the system calculates:

```
Noise = Maximum Reading − Minimum Reading
```

This provides a better estimate of environmental sound intensity.

---

## Sensor Error Detection

The code checks whether the DHT sensor returns valid data.

```cpp
if(isnan(temp) || isnan(hum))
```

If an error occurs:

- LCD displays an error message
- Invalid data is ignored

---

#  Air Quality Classification

| MQ135 Value | Status |
|-------------|---------|
| < 1200 | GOOD |
| 1200 – 2199 | MODERATE |
| ≥ 2200 | POOR |

> **Note:** These thresholds are demonstration values and should be calibrated for accurate environmental measurements.

---

#  Advantages

- Simple and low-cost
- Easy to build
- Real-time monitoring
- Modular and maintainable code
- Expandable for IoT applications
- Low power consumption
- User-friendly LCD display

---

#  Limitations

- DHT11 has limited accuracy.
- MQ135 requires calibration for precise gas concentration measurements.
- Noise readings indicate relative intensity rather than calibrated decibels (dB).
- Data is displayed locally and not stored or transmitted.

---

# Future Enhancements

- Wi-Fi-based cloud monitoring
- Mobile application
- Blynk or MQTT integration
- ThingSpeak dashboard
- Email/SMS alerts
- Buzzer for hazardous conditions
- SD card data logging
- Real-time clock (RTC)
- GPS-based environmental mapping
- AI-based pollution prediction

---

#  Technologies Used

- ESP32
- Arduino IDE
- Embedded C++
- I2C Communication
- Analog-to-Digital Conversion (ADC)
- DHT11 Sensor
- MQ135 Sensor
- Sound Sensor

---

# Applications

- Smart Cities
- Environmental Monitoring
- Schools and Colleges
- Industries
- Laboratories
- Offices
- Smart Homes
- Pollution Monitoring
- Research Projects
- IoT Demonstrations

---

# 🤝 Contributors

- Himanshu chaudhary
- Pulkit Singh
- Rishi Chaudhary
- Rudra kumar pathak

---

# 📄 License

This project is developed for educational and hackathon purposes. You are free to use and modify it with proper attribution.

---

# ⭐ Acknowledgements

Special thanks to:

- Arduino Community
- ESP32 Open Source Community
- Adafruit Libraries
- Open-source IoT developers

---

## Made with ❤️ using ESP32
