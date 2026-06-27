# Week 3: Sensors & Actuators

**Questions Covered**: Q21–Q30

This module covers sensor integration, data logging, actuator control, and embedded systems programming with real-world IoT applications. You will read temperature/humidity, measure distance, detect light and motion, control motors and servos, and implement user authentication systems.

---

## Module Checklist

- [ ] Q21: DHT11 Weather Station
- [ ] Q22: HC-SR04 Ultrasonic Parking Sensor (Non-Blocking)
- [ ] Q23: Smart Street Light (LDR + PIR + Event Logging)
- [ ] Q24: Multi-Sensor Logger (DHT + LDR + HC-SR04)
- [ ] Q25: Servo Control with Potentiometer
- [ ] Q26: DC Motor Control with L298N
- [ ] Q27: Relay Control with Hysteresis
- [ ] Q28: 4x4 Keypad + 16x2 LCD Password System
- [ ] Q29: Theory: Sensor Calibration & Two-Point Method
- [ ] Q30: Theory: I2C Protocol & Common I2C Sensors

---

## Key Learnings

- **Q21–Q24**: Multi-sensor data acquisition, CSV logging, formatted output
- **Q25–Q27**: Actuator control (servo, motor, relay) with feedback
- **Q28**: User interface design (keypad input, LCD display, state management)
- **Q29–Q30**: Sensor fundamentals and communication protocols

---

## Folder Structure

```
week3/
├── README.md                                    # This file
├── theory_answers.md                            # Q29 & Q30 theory responses
├── data/
│   ├── sample_readings.csv                      # Q21: Example DHT11 data
│   └── sensor_log.txt                           # Q24: Multi-sensor log samples
├── schematics/
│   └── README.md                                # Q23: Circuit diagram placeholder
├── dht11_weather_station/
│   ├── dht11_weather_station.ino                # Q21: Temp/humidity logging
│   └── README.md                                # Library version, calibration notes
├── ultrasonic_parking_sensor/
│   ├── ultrasonic_parking_sensor.ino            # Q22: Non-blocking HC-SR04
│   └── README.md                                # Timing, distance formula, millis() explanation
├── smart_street_light/
│   ├── smart_street_light.ino                   # Q23: LDR + PIR + timestamp logging
│   └── README.md                                # Event logging, brightness control
├── multi_sensor_logger/
│   ├── multi_sensor_logger.ino                  # Q24: DHT + LDR + HC-SR04
│   └── README.md                                # Sensor block format reference
├── servo_control/
│   ├── servo_control.ino                        # Q25: Potentiometer → servo angle
│   └── README.md                                # Servo timing, angle mapping
├── dc_motor_l298n/
│   ├── dc_motor_l298n.ino                       # Q26: Speed/direction control
│   └── README.md                                # L298N pin mapping, PWM speed control
├── relay_ac_simulation/
│   ├── relay_ac_simulation.ino                  # Q27: DHT-driven relay with hysteresis
│   └── README.md                                # Hysteresis logic, relay chatter prevention
└── keypad_lcd_access/
    ├── keypad_lcd_access.ino                    # Q28: Password system with lockout
    └── README.md                                # Keypad wiring, LCD commands, PIN configuration
```

---

## Hardware Overview

| Sensor/Actuator | Questions | Libraries | Pins |
|-----------------|-----------|-----------|------|
| DHT11 | Q21, Q24 | DHT (Adafruit) | Data pin (any digital) |
| HC-SR04 | Q22, Q24 | None (timing-based) | Trig (digital), Echo (digital) |
| LDR | Q23, Q24 | None (analogRead) | A0-A5 |
| PIR Motion | Q23 | None (digitalRead) | Any digital pin |
| Servo | Q25 | Servo | PWM pin (3, 5, 6, 9, 10, 11) |
| DC Motor | Q26 | None | L298N: IN1, IN2 (direction), ENA (PWM speed) |
| Relay | Q27 | None | Any digital pin (5V relay) |
| 4x4 Keypad | Q28 | Keypad (Mark Stanley) | 8 pins (4 rows + 4 cols) |
| 16x2 LCD | Q28 | LiquidCrystal | 6+ pins (I2C option reduces to 2 pins) |

---

## Data Logging Standards

### Q21 CSV Format (DHT11):
```
timestamp,temp_C,temp_F,humidity
0,26.5,79.7,58
2000,26.8,80.2,59
4000,27.1,80.8,60
```

### Q24 Sensor Block Format:
```
=== SENSOR LOG ===
Time : 5000 ms
Temp : 27 C | Humidity: 60%
Light : 72% (Bright)
Distance : 45 cm
==================
```

---

## Tips for Success

1. **Test sensors individually first**: Verify each sensor works before combining
2. **Use millis() for timing**: Especially Q22, Q23, Q24 for non-blocking reads
3. **Timestamp all events**: Helps with debugging and IoT analytics
4. **Validate input**: Q28 keypad input validation crucial
5. **Document calibration**: Record sensor min/max values for two-point calibration (Q29)
6. **Handle edge cases**: Sensor errors, relay chatter, password lockout

---

## Bonus Marks

- **Q22**: +1 mark for fully non-blocking millis()-based timing (no delay() calls)
- **Q23**: Event logging with timestamp formatting
- **Q28**: Successfully implementing 3-strike lockout without delay()

---

## Next Steps

1. **Q21**: Read DHT11 every 2s, log to Serial in CSV format
2. **Q22**: Measure distance with HC-SR04 using non-blocking timing
3. **Q23**: Implement day/night and motion-triggered lighting
4. **Q24**: Combine all sensors in one comprehensive logger
5. **Q25–Q27**: Control servo, motor, and relay based on inputs
6. **Q28**: Build a secure keypad password entry system
7. **Q29–Q30**: Complete sensor calibration and I2C protocol theory

---

**Recommended Reading**:
- [DHT11 Sensor Guide](https://www.circuitbasics.com/how-to-set-up-the-dht11-humidity-sensor-on-an-arduino/)
- [HC-SR04 Ultrasonic Sensor](https://www.electronicshub.org/ultrasonic-sensor-arduino-tutorial/)
- [Arduino Servo Control](https://www.arduino.cc/reference/en/libraries/servo/)
- [L298N Motor Driver](https://howtomechatronics.com/tutorials/arduino/arduino-dc-motor-control-tutorial-l298n-pwm/)
- [I2C Protocol](https://i2c.info/)
