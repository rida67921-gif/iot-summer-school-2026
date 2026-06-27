# Q29 & Q30: Sensor Theory

## Q29: Sensor Calibration & Two-Point Calibration Method

### **What is Sensor Calibration?**

Sensor calibration is the process of adjusting a sensor's output to match known, accurate reference values. In IoT systems, calibration ensures:
- **Accuracy**: Sensor readings match real-world values
- **Consistency**: Multiple sensors read consistently
- **Reliability**: Compensate for sensor drift and manufacturing variation

Without calibration, a potentiometer might read 515 when it's actually at 50%, or a soil moisture sensor might give wildly inaccurate percentages.

### **Why Calibration Matters in IoT**

1. **Data Quality**: Inaccurate sensor data leads to poor decisions (e.g., overwatering plants)
2. **System Reliability**: Calibrated systems are trustworthy for production deployments
3. **User Experience**: Accurate readings build user confidence
4. **Cost Efficiency**: Prevent wasting resources on miscalibrated sensor feedback

**Real-World Example**: A soil moisture sensor must be calibrated to know what "50% moisture" actually means for a specific soil type.

---

### **Two-Point Calibration Method**

The two-point calibration method is the most practical approach for Arduino projects. It uses two known reference values to create a linear equation that maps raw sensor readings to calibrated values.

#### **Steps:**

1. **Collect Reference Point 1** (Dry/Low condition)
   - Place sensor in **completely dry air** or **minimum condition**
   - Record the raw ADC reading: `reading_dry`
   - Record the true value: `value_dry` (e.g., 0%)

2. **Collect Reference Point 2** (Wet/High condition)
   - Place sensor in **saturated water** or **maximum condition**
   - Record the raw ADC reading: `reading_wet`
   - Record the true value: `value_wet` (e.g., 100%)

3. **Calculate the Linear Relationship**
   ```
   calibrated_value = ((raw_reading - reading_dry) / (reading_wet - reading_dry)) × (value_wet - value_dry) + value_dry
   ```

4. **Implement in Code**
   ```cpp
   const int SENSOR_DRY = 400;    // Raw value in dry air
   const int SENSOR_WET = 950;    // Raw value in water
   
   int raw = analogRead(A0);
   int moisture_percent = map(raw, SENSOR_DRY, SENSOR_WET, 0, 100);
   ```

#### **Example: Soil Moisture Sensor Calibration**

```
Step 1: Dry calibration
  - Completely dry soil in air
  - Raw reading: 200 ADC
  - True value: 0% (bone dry)

Step 2: Wet calibration
  - Sensor submerged in water
  - Raw reading: 950 ADC
  - True value: 100% (fully saturated)

Step 3: Linear equation
  moisture% = ((raw - 200) / (950 - 200)) × (100 - 0) + 0
  moisture% = ((raw - 200) / 750) × 100

Step 4: Test
  - Raw reading 575: moisture% = ((575 - 200) / 750) × 100 = 50%
  - Raw reading 350: moisture% = ((350 - 200) / 750) × 100 = 20%
```

#### **Arduino Implementation:**

```cpp
// Calibration constants (determined experimentally)
const int SOIL_MOISTURE_DRY = 200;
const int SOIL_MOISTURE_WET = 950;

void loop() {
  int raw_reading = analogRead(A0);
  
  // Apply two-point calibration
  int moisture_percent = map(raw_reading, SOIL_MOISTURE_DRY, SOIL_MOISTURE_WET, 0, 100);
  
  // Constrain to 0-100% range
  moisture_percent = constrain(moisture_percent, 0, 100);
  
  Serial.print("Raw: ");
  Serial.print(raw_reading);
  Serial.print(" → Moisture: ");
  Serial.print(moisture_percent);
  Serial.println("%");
}
```

### **Tips for Two-Point Calibration:**

- **Choose extreme conditions**: Dry/wet, dark/bright, cold/hot for maximum accuracy
- **Record multiple readings**: Average 5-10 readings at each calibration point
- **Document calibration**: Write down your `_DRY` and `_WET` values in code comments
- **Recalibrate periodically**: Sensor sensitivity can drift over months/years
- **Account for temperature**: Some sensors (DHT11) have temperature-dependent outputs
- **Environmental factors**: Light, humidity, and dust affect LDR readings

### **Real IoT Example: Smart Plant Watering System**

```cpp
// Calibrate for your specific soil
const int SOIL_DRY = 180;    // Bone-dry potted soil
const int SOIL_WET = 920;    // Fully saturated soil
const int WATER_THRESHOLD = 40;  // Water when <40% moisture

void loop() {
  int raw = analogRead(SOIL_SENSOR);
  int moisture = map(raw, SOIL_DRY, SOIL_WET, 0, 100);
  
  if (moisture < WATER_THRESHOLD) {
    activate_pump();  // Water the plant
    Serial.println("Plant too dry - watering...");
  }
}
```

---

---

## Q30: I2C Protocol & Common I2C Sensors

### **What is I2C (Inter-Integrated Circuit)?**

I2C is a **two-wire serial communication protocol** for microcontrollers to communicate with sensors, displays, and other peripheral devices. It's one of the most popular protocols in embedded systems and IoT.

### **I2C Overview:**

- **Two signal lines:**
  - **SDA (Serial Data)** – bidirectional data line (pulled high by resistors)
  - **SCL (Serial Clock)** – clock synchronization line
- **Multi-master/multi-slave**: Multiple devices can communicate on the same bus
- **7-bit or 10-bit addressing**: Each device has a unique address (0x00–0x7F typically)
- **Speed**: 100 kHz (Standard), 400 kHz (Fast), 1 MHz (Fast+)

### **How I2C Works (Master-Slave Concept):**

```
Master (Arduino)                Bus (SDA + SCL)                Slave Devices
                                                          BMP280 (address 0x76)
                                                          MPU6050 (address 0x68)
                                                          DS3231 (address 0x68)

1. Master sends START condition
2. Master sends device address + R/W bit
3. Addressed slave responds with ACK (acknowledge)
4. Master reads/writes data (8-bit bytes)
5. Master sends STOP condition
```

### **7-Bit Address Format:**

```
I2C Address = 0x68 (example for MPU6050)
Binary: 0110 1000
7-bit:  0110 100  (actual address)
Bit 0:  0 (write) or 1 (read)
```

### **I2C Pins on Arduino UNO:**

- **SDA**: Analog Pin 4 (A4)
- **SCL**: Analog Pin 5 (A5)
- **Pull-up Resistors**: Typically 4.7kΩ resistors connect SDA and SCL to 5V (often built-in on modules)

### **Arduino Code Example (Reading from I2C device):**

```cpp
#include <Wire.h>

const int MPU_ADDRESS = 0x68;  // MPU6050 default address

void setup() {
  Serial.begin(9600);
  Wire.begin();  // Join I2C bus
}

void loop() {
  Wire.beginTransmission(MPU_ADDRESS);
  Wire.write(0x3B);  // Register address for acceleration X
  Wire.endTransmission();
  
  // Request 2 bytes from MPU6050
  Wire.requestFrom(MPU_ADDRESS, 2);
  
  if (Wire.available() >= 2) {
    int16_t accel_x = (Wire.read() << 8) | Wire.read();
    Serial.println(accel_x);
  }
  
  delay(100);
}
```

---

### **Three Common I2C Sensors (Accurate Real Examples):**

#### **1. BMP280 (Barometric Pressure & Temperature)**
- **Address**: 0x76 or 0x77 (selectable)
- **What it measures**: Atmospheric pressure, altitude, temperature
- **Library**: Adafruit BMP280
- **Use Case**: Weather stations (Q21-like), altitude tracking, indoor/outdoor air pressure
- **Output**: Pressure (Pa), Temperature (°C)

```cpp
#include <Adafruit_BMP280.h>
Adafruit_BMP280 bmp280;

void setup() {
  bmp280.begin(0x76);  // I2C address
}

void loop() {
  float pressure = bmp280.readPressure();
  float temp = bmp280.readTemperature();
  float altitude = bmp280.readAltitude(1013.25);  // Sea level pressure
}
```

#### **2. MPU6050 (Accelerometer & Gyroscope)**
- **Address**: 0x68 or 0x69 (selectable)
- **What it measures**: 3-axis acceleration, 3-axis angular velocity, temperature
- **Library**: MPU6050 (by Jeff Rowberg)
- **Use Case**: Motion detection, robotics, drone stabilization, gesture recognition
- **Output**: Acceleration (m/s²), Rotation (°/s)

```cpp
#include "MPU6050.h"
MPU6050 mpu6050;

void setup() {
  mpu6050.initialize();
}

void loop() {
  int16_t ax = mpu6050.getAccelerationX();
  int16_t gy = mpu6050.getRotationY();
  int16_t temp = mpu6050.getTemperature();
}
```

#### **3. DS3231 (Real-Time Clock with Temperature)**
- **Address**: 0x68 (fixed)
- **What it measures**: Date, time (with ±2 minute/year accuracy), temperature
- **Library**: RTClib (by Adafruit)
- **Use Case**: Data logging with timestamps, scheduled tasks, IoT event timing
- **Output**: Year/Month/Day/Hour/Minute/Second, Temperature (°C)

```cpp
#include "RTClib.h"
RTC_DS3231 rtc;

void setup() {
  rtc.begin();
  // Optional: set time if not set
  // rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
}

void loop() {
  DateTime now = rtc.now();
  Serial.print(now.hour());
  Serial.print(":");
  Serial.print(now.minute());
  Serial.print(":")
  Serial.println(now.second());
  
  float temp = rtc.getTemperature();
}
```

### **I2C Advantages for IoT:**

- **Simple wiring**: Only 2 wires (SDA, SCL) vs. multiple pins per sensor
- **Scalable**: Add multiple sensors without extra pins
- **Standardized**: Consistent protocol across thousands of devices
- **Reliable**: Built-in error checking (ACK/NACK)

### **I2C Limitations:**

- **Short distance**: Typically <3m (use I2C extender for longer distances)
- **Speed trade-off**: Multiple devices slow down communication
- **Address conflicts**: If two devices have the same address, use I2C multiplexer
- **Pull-up resistors**: Must be present on SDA/SCL lines

### **How to Identify I2C Devices:**

```cpp
// I2C Scanner sketch (run to find all devices on bus)
#include <Wire.h>

void setup() {
  Wire.begin();
  Serial.begin(9600);
  Serial.println("I2C Scanner...");
}

void loop() {
  for (int address = 1; address < 127; address++) {
    Wire.beginTransmission(address);
    if (Wire.endTransmission() == 0) {
      Serial.print("Found device at 0x");
      Serial.println(address, HEX);
    }
  }
  delay(5000);
}
```

### **Real-World IoT Example: Comprehensive Sensor Hub**

```cpp
// Single I2C bus with 3 devices:
// BMP280 (0x76): Pressure, altitude, temp
// MPU6050 (0x68): Motion detection
// DS3231 (0x68*): Real-time clock with timestamp
// *Note: DS3231 and MPU6050 conflict - use address selector or multiplexer

void setup() {
  Wire.begin();  // Initialize I2C
  bmp280.begin(0x76);
  mpu6050.initialize();
  rtc.begin();
}

void loop() {
  // All sensors communicate on same 2-wire bus
  DateTime now = rtc.now();
  float pressure = bmp280.readPressure();
  int16_t accel = mpu6050.getAccelerationX();
  
  // Log everything with timestamp
  Serial.print(now.unixtime());
  Serial.print(",");
  Serial.print(pressure);
  Serial.print(",");
  Serial.println(accel);
}
```

---

## Summary

| Topic | Key Concept |
|-------|----------|
| **Calibration** | Two-point method creates linear mapping from raw → calibrated values |
| **I2C Basics** | 2-wire (SDA, SCL) serial protocol with 7-bit addresses |
| **Master/Slave** | Arduino is master; sensors are slaves responding to addresses |
| **Common Sensors** | BMP280 (pressure), MPU6050 (motion), DS3231 (time) |
| **Advantages** | Scalable, simple wiring, standardized protocol |

---

**Author**: [YOUR NAME]  
**Date**: [DATE]  
**Questions**: Q29, Q30
