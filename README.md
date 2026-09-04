 # EXPERIMENT-03-INTERFACING-DIGITAL-SENSOR-DHT-11-Temperature-Sensor-and-Rain-Sensor-WITH-EDGE-DEVELOPMENT-BOARD-

---

### **NAME:** T.Rishabh srivatsav
### **DEPARTMENT:**  CSE(IOT)
### **ROLL NO:**  212224113001
### **DATE OF EXPERIMENT:**  05/08/2026

---

## **AIM:**  
To interface an **Temperature and humidity sensor (DHT 11) Rain Sensor (LM393)** with the **Raspberry Pi 4** and display the sensor readings using HiveMQ cloud.

---

## **APPARATUS REQUIRED:**  
1. Raspberry Pi 4  
2. Temperature and Humidity sensor (DHT-11) and Rain Sensor (LM393)  
3. Jumper Wires  
4. Breadboard  
5. USB Cable  
6. Computer with Thonny IDE  

---

## **THEORY:**  
<img width="1293" height="744" alt="image" src="https://github.com/user-attachments/assets/3c04afa6-1517-45d2-88f1-e671d9ed1ffb" />

 ### FIGURE-01 RASPI PI 4 PINOUT DIAGRAM: 

The Raspberry Pi 4 Model B is built around a Broadcom BCM2711 system-on-chip that integrates a quad-core ARM Cortex-A72 (64-bit) CPU, VideoCore VI GPU, memory controller, and peripheral interfaces, forming a compact yet complete computer architecture where the SoC connects internally to RAM, USB 3.0 controller, Gigabit Ethernet, HDMI display, and wireless modules. Its 40-pin GPIO header provides a flexible pin configuration consisting of power pins (5 V and 3.3 V), multiple ground pins, and general-purpose input/output pins that operate at 3.3 V logic and can be programmed for digital I/O or alternate functions. Key alternate functions include I²C (SDA, SCL) for sensor communication, SPI (MOSI, MISO, SCLK, CS) for high-speed peripheral interfacing, UART (TX, RX) for serial communication, and PWM for control applications.  For communication, I2C (SDA, SCL), SPI (MOSI, MISO, SCK), and UART (TX, RX) interfaces are mapped across different GPIO pins, allowing seamless connectivity with sensors and peripherals. All GPIO pins support PWM (Pulse Width Modulation), making it useful for motor control, LED brightness adjustment, and sound applications. The BOOTSEL button enables USB mass storage mode for firmware flashing, while the DEBUG pins (SWD interface) provide debugging capabilities. With its low power consumption, flexible GPIO options, and rich interface support, the Raspberry Pi Pico is widely used for IoT, embedded systems, robotics, and automation projects.This architecture and pin multiplexing allow the Raspberry Pi 4 to act as both a general-purpose computing platform and an embedded controller, supporting rapid prototyping, hardware interfacing, and IoT applications.
## Temperature and Humidity Sensor (DHT-11):
The DHT11 is a low-cost digital sensor used to measure ambient temperature and relative humidity in embedded and IoT applications. It integrates a thermistor for temperature sensing and a capacitive humidity sensor for detecting moisture levels in the air, along with an internal 8-bit microcontroller that processes the signals and provides calibrated digital output through a single-wire communication interface. The sensor operates typically at 3.3 V to 5 V, measures temperature in the range of 0 °C to 50 °C with ±2 °C accuracy, and humidity from 20% to 80% with ±5% accuracy. Due to its simple interface, low power consumption, and reliable performance, it is widely used in weather monitoring systems, home automation, agricultural monitoring, and basic environmental data acquisition projects.

<img width="1000" height="1000" alt="image" src="https://github.com/user-attachments/assets/5c8d35b5-4381-434f-8617-db4b8fe19154" />

### FIGURE-02 Temperature and Humidity Sensor (DHT-11)

## Rain Sensor (LM393):
A rain sensor is one kind of switching device which is used to detect the rainfall. It works like a switch and the working principle of this sensor is, whenever there is rain, the switch will be normally closed. The rain sensor module/board is shown below. Basically, this board includes nickel coated lines and it works on the resistance principle. This sensor module permits to gauge moisture through analog output pins & it gives a digital output while moisture threshold surpasses. This module is similar to the LM393 IC because it includes the electronic module as well as a PCB. Here PCB is used to collect the raindrops. When the rain falls on the board, then it creates a parallel resistance path to calculate through the operational amplifier. This sensor is a resistive dipole, and based on the moisture only it shows the resistance. For example, it shows more resistance when it is dry and shows less resistance when it is wet..
<img width="525" height="240" alt="image" src="https://github.com/user-attachments/assets/fdf4df13-4659-46ed-bbab-44bbae5a360f" />

<img width="443" height="266" alt="image" src="https://github.com/user-attachments/assets/c9750628-c41f-4181-8dcd-e07873d227d9" />

<img width="393" height="120" alt="image" src="https://github.com/user-attachments/assets/997aa631-9647-42cc-94d6-754742f7bf18" />


 ### FIGURE-03 Rain Sensor (LM393) Rain Sensor Module & connection diagram

## Working Principle:
Experiment 3A
The Temperature and Humidity Sensor (DHT-11) OUT is connected to one of the GPIO pins of the Raspberry Pi 4.
The Python script sets the measure the real time temperature and Humidity output and shown in HiveMQcloud with current status and Console.
CIRCUIT DIAGRAM
Connect the Vcc of the Temperature and Humidity Sensor (DHT-11) is connected to +5V in Raspberrry Pi4.
Connect the Gnd of the Temperature and Humidity Sensor (DHT-11) is connected to Gnd in Raspberrry Pi4.
Connect the OUT to any one GPIO.


Experiment 3B
The Rain Sensor (LM393) D0 is connected one of the GPIO pins in Raspberry Pi 4.
The Python script sets the Rain Sensor (LM393) value based on the variation in the rain water (Dry or Wet) and shown in HiveMQ Cloud and console.
CIRCUIT DIAGRAM
Connect the Rain Sensor (LM393) Vcc to any +5V.
Connect the Rain Sensor (LM393) GND to any GND.
Connect the Rain Sensor (LM393) D0 to any one GPIO. 


Experiment 3A
## PROGRAM (Python)
```


 import Adafruit_DHT
import paho.mqtt.client as mqtt
import ssl
import time

# ---------------- DHT11 Setup ----------------

DHT_SENSOR = Adafruit_DHT.DHT11
DHT_PIN = 18  # GPIO18

# ---------------- HiveMQ Cloud Credentials ----------------

MQTT_BROKER = "99abf9ad843d4d2c9de4899b56eb863a.s1.eu.hivemq.cloud"
MQTT_PORT = 8883
MQTT_USER = "hivemq.webclient.1785579047827"
MQTT_PASSWORD = "u;2%rC70tVIM,n<1hfWT"

TEMP_TOPIC = "raspberrypi/dht/temperature"
HUM_TOPIC = "raspberrypi/dht/humidity"

# ---------------- MQTT Client Setup ----------------

client = mqtt.Client()
client.username_pw_set(MQTT_USER, MQTT_PASSWORD)

client.tls_set(tls_version=ssl.PROTOCOL_TLS)

client.connect(MQTT_BROKER, MQTT_PORT)

print("Connected to HiveMQ Cloud")
print("Reading DHT11 Sensor...\n")

# ---------------- Main Loop ----------------

while True:

    humidity, temperature = Adafruit_DHT.read(DHT_SENSOR, DHT_PIN)

    if humidity is not None and temperature is not None:

        print(f"Temperature = {temperature} °C")
        print(f"Humidity = {humidity} %")
        print("---------------------------")

        # Publish to HiveMQ
        client.publish(TEMP_TOPIC, temperature)
        client.publish(HUM_TOPIC, humidity)

        print("Data sent to HiveMQ\n")

    else:
        print("Sensor failure. Check wiring.")

    time.sleep(10)
 
````

### OUPUT  
# FIGURE -04 ADD TITILE HERE 
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/a6e7e277-59a0-49d5-b51d-034f0f607fee" />

#  FIGURE -05 ADD TITILE HERE 
<img width="546" height="341" alt="image" src="https://github.com/user-attachments/assets/2647ac3b-0286-4e10-873f-555e46f7fb9c" />

# FIGURE -06 ADD TITLE HERE 
<img width="1862" height="917" alt="image" src="https://github.com/user-attachments/assets/5e3dc22a-d330-4b67-af12-a0b7ea7628c5" />

Experiment 3B
## PROGRAM (Python)
```
import time
import ssl
import json
import RPi.GPIO as GPIO
import paho.mqtt.client as mqtt

# =====================================================
# GPIO SETUP
# =====================================================

GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)

RAIN_SENSOR_PIN = 18

GPIO.setup(RAIN_SENSOR_PIN, GPIO.IN)

# =====================================================
# MQTT SETUP
# =====================================================

MQTT_BROKER = "99abf9ad843d4d2c9de4899b56eb863a.s1.eu.hivemq.cloud"
MQTT_PORT = 8883

MQTT_USER = "hivemq.webclient. 1785581075112"
MQTT_PASSWORD = ".WszV%830apZI@X?ye4G"

MQTT_TOPIC = "raspberrypi/rain"

client = mqtt.Client()

client.username_pw_set(
    MQTT_USER,
    MQTT_PASSWORD
)

client.tls_set(
    tls_version=ssl.PROTOCOL_TLS
)

# =====================================================
# CONNECT TO HIVEMQ
# =====================================================

print("Connecting to HiveMQ Cloud...")

client.connect(
    MQTT_BROKER,
    MQTT_PORT
)

client.loop_start()

print("Connected Successfully")

# =====================================================
# MAIN LOOP
# =====================================================

try:

    while True:

        rain_value = GPIO.input(RAIN_SENSOR_PIN)

        # ACTIVE LOW SENSOR
        if rain_value == 0:

            status = "RAIN DETECTED"
            rain_status = 1

        else:

            status = "NO RAIN"
            rain_status = 0

        print(status)

        payload = {
            "rain_status": rain_status,
            "message": status
        }

        client.publish(
            MQTT_TOPIC,
            json.dumps(payload)
        )

        print("Data Published")
        print(payload)

        time.sleep(5)

except KeyboardInterrupt:

    print("Program Stopped")

finally:

    GPIO.cleanup()

    client.loop_stop()
    client.disconnect()

    print("GPIO Cleaned")
    print("MQTT Disconnected")
 
````

### OUPUT  

# FIGURE -07 ADD TITILE HERE 
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/ec403f50-6541-48b8-a14b-f7bcdf9f842c" />

#  FIGURE -08 ADD TITILE HERE 
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/915f9ef3-12e7-4202-8257-38e4d5285b05" />

# FIGURE -09 ADD TITLE HERE 

<img width="883" height="436" alt="Screenshot 2026-09-04 192955" src="https://github.com/user-attachments/assets/0cce8a8b-de19-4da8-a035-99f650d43aa9" />




## **RESULT:**  
The **Temperature and humidity sensor (DHT 11) Rain Sensor (LM393)** was successfully interfaced with the **Raspberry Pi 4**, and real-time **Temperature, Humidity and Rain status** were read and displayed in Console and HiveMq Cloud. 

---

