 # EXPERIMENT-03-INTERFACING-DIGITAL-SENSOR-WITH-EDGE-DEVELOPMENT-BOARD-

---

### **NAME:**   Kamali E
### **DEPARTMENT:**   CSE - IOT
### **ROLL NO:**  212222110015
### **DATE OF EXPERIMENT:**  27.03.2025

---

## **AIM:**  
To interface an **MPU6050 6-Axis Accelerometer & Gyroscope Sensor** with the **Raspberry Pi Pico** and display the sensor readings using MicroPython.

---

## **APPARATUS REQUIRED:**  
1. Raspberry Pi Pico  
2. MPU6050 6-Axis Accelerometer & Gyroscope Sensor  
3. Jumper Wires  
4. Breadboard  
5. USB Cable  
6. Computer with Thonny IDE  

---

## **THEORY:**  
### **About Raspberry Pi Pico:**  
Raspberry Pi Pico is a microcontroller board based on the **RP2040 chip**. It features:  
- Dual-core ARM Cortex-M0+ processor  
- 26 multi-function GPIO pins  
- Supports **MicroPython** and **C/C++**  
- Interfaces like **I2C, SPI, UART, and PWM**  
- Low power consumption, ideal for **IoT applications**  

### **About MPU6050 Sensor:**  
The **MPU6050** is a **6-Axis Inertial Measurement Unit (IMU)** that includes:  
- **3-axis accelerometer** and **3-axis gyroscope**  
- **I2C communication protocol** for easy interfacing  
- **Operating Voltage:** 3.3V – 5V  
- **Accelerometer Range:** ±2g, ±4g, ±8g, ±16g  
- **Gyroscope Range:** ±250°/s, ±500°/s, ±1000°/s, ±2000°/s  

The **accelerometer** measures linear acceleration in **X, Y, Z axes**, while the **gyroscope** measures rotational velocity. The sensor communicates with the Raspberry Pi Pico via **I2C protocol**.

---

## **WORKING PRINCIPLE:**  
1. The **MPU6050 sensor** is connected to the **Raspberry Pi Pico** using the **I2C communication protocol**.  
2. The **Pico reads acceleration and gyroscope values** from the sensor registers.  
3. The data is **processed and displayed on the serial monitor**.  
4. The readings can be used for **motion tracking, tilt sensing, or gesture recognition**.

---

## **CIRCUIT DIAGRAM:**  
### **Connections:**  

| MPU6050 Pin | Raspberry Pi Pico Pin |
|------------|----------------------|
| VCC | 3.3V |
| GND | GND |
| SDA | GP20 |
| SCL | GP21 |

---

## **PROGRAM (MicroPython)**  
```

    from machine import Pin, I2C
import utime

# MPU6050 I2C address
MPU6050_ADDR = 0x68

# MPU6050 Registers
PWR_MGMT_1 = 0x6B
ACCEL_XOUT_H = 0x3B
GYRO_XOUT_H = 0x43

# Initialize I2C
sda = Pin(20)  # Define your SDA pin
scl = Pin(21)  # Define your SCL pin
i2c = I2C(1, scl=scl, sda=sda, freq=400000)  # Use I2C1

def mpu6050_init():
    i2c.writeto_mem(MPU6050_ADDR, PWR_MGMT_1, b'\x00')  # Wake up MPU6050

def read_raw_data(reg):
    data = i2c.readfrom_mem(MPU6050_ADDR, reg, 2)
    value = (data[0] << 8) | data[1]  # Combine high and low bytes
    if value > 32767:
        value -= 65536  # Convert to signed 16-bit
    return value

def get_sensor_data():
    accel_x = read_raw_data(ACCEL_XOUT_H) / 16384.0  # Convert to g
    accel_y = read_raw_data(ACCEL_XOUT_H + 2) / 16384.0
    accel_z = read_raw_data(ACCEL_XOUT_H + 4) / 16384.0
    
    gyro_x = read_raw_data(GYRO_XOUT_H) / 131.0  # Convert to deg/s
    gyro_y = read_raw_data(GYRO_XOUT_H + 2) / 131.0
    gyro_z = read_raw_data(GYRO_XOUT_H + 4) / 131.0
    
    return (accel_x, accel_y, accel_z, gyro_x, gyro_y, gyro_z)

# Initialize MPU6050
mpu6050_init()

while True:
    ax, ay, az, gx, gy, gz = get_sensor_data()
    print(f"Accel: X={ax:.2f}g, Y={ay:.2f}g, Z={az:.2f}g | Gyro: X={gx:.2f}°/s, Y={gy:.2f}°/s, Z={gz:.2f}°/s")
    utime.sleep(1)
```



## **OUTPUT:**  

![image](https://github.com/user-attachments/assets/826e2749-f65e-4a62-b864-4033ee8fe9bf)

![image](https://github.com/user-attachments/assets/c18d7630-d934-4006-a96f-391a5f1734ae)

![image](https://github.com/user-attachments/assets/eca06fb2-7e28-4f62-a48f-b0939bb4596e)

![image](https://github.com/user-attachments/assets/70ff02f2-fc0f-450e-afec-74ccb69a5053)


## **RESULT:**  
The **MPU6050 sensor** was successfully interfaced with the **Raspberry Pi Pico**, and real-time **acceleration and gyroscope data** were read and displayed. The sensor values can be used for **motion tracking, tilt detection, and gesture control applications**.

---

