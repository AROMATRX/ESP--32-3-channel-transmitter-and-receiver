# ESP--32-3-channel-transmitter-and-receiver
🔧 ES-32 3-Channel Transmitter &amp; Receiver A simple and efficient 3-channel communication system using two ESP32 boards via ESP-NOW protocol. Ideal for wireless control of RC models, robots, Drones or IoT devices.
 
# ES--32-3-Channel-Transmitter-and-Receiver

A fully wireless 3-channel communication system using two ESP32 boards and the ESP-NOW protocol — ideal for remote control applications like RC cars, robots, and automation projects.

---

## 📦 Project Structure

Folder contains:

* `transmm.ino` (Transmitter code)
* `RESS.ino` (Receiver code)
* `Get_MAC_Address.ino` (To get MAC address of receiver)
* `Transmitter.png` (Transmitter circuit diagram)
* `Receiver.png` (Receiver circuit diagram)

---

## 🛁 How ESP-NOW Works

ESP-NOW is a wireless protocol by Espressif that allows ESP32 boards to communicate without Wi-Fi or internet. It's fast and reliable, perfect for real-time transmission.

---

## 🧰 Hardware Required

| Component                 | Quantity |
| ------------------------- | -------- |
| ESP32 WROOM Dev Board     | 2        |
| Tactile Push Buttons      | 4        |
| Analog Joystick Module    | 1        |
| Potentiometer (5kΩ)       | 1        |
| Micro Servo Motors (SG90) | 3        |
| Jumper Wires              | Many     |
| Breadboards (optional)    | 2        |

---

## 📍 Wiring Diagrams

### 🕦 Transmitter Setup

![Transmitter Wiring](Transmitter.png)

* Buttons:

  * LEFT/RIGHT = GPIO 21 / GPIO 22
  * THROTTLE/STEERING = GPIO 18 / GPIO 19
* Joystick:

  * X-Axis = GPIO 34
  * Y-Axis = GPIO 35
* Potentiometer = GPIO 32

### 🔳 Receiver Setup

![Receiver Wiring](Receiver.png)

* Servo Channels:

  * CH1 (e.g., Steering) = GPIO 14
  * CH2 (e.g., Throttle) = GPIO 27
  * CH3 (e.g., Auxiliary) = GPIO 26

> ⚠️ **Note:** Do not power servos from ESP32 3.3V. Use external 5V supply or 5V from your ESP32 VIN if available.

---

## 🔄 Step-by-Step Setup

### 🧪 STEP 1: Get the Receiver MAC Address

1. Open `Get_MAC_Address.ino` in Arduino IDE
2. Select the correct COM port and ESP32 board
3. Upload and open **Serial Monitor**
4. Copy the MAC address (e.g., `24:6F:28:AB:CD:EF`)

```cpp
void setup() {
  Serial.begin(115200);
  WiFi.mode(WIFI_STA);
  Serial.println(WiFi.macAddress());
}
```

---

### 🔧 STEP 2: Add Receiver MAC Address to Transmitter Code

To send data to Receiver, you must add its MAC address into Transmitter code:

1. Open `transmm.ino` in Arduino IDE.
2. Locate the line:

```cpp
uint8_t broadcastAddress[] = {0x24, 0x6F, 0x28, 0xAB, 0xCD, 0xEF};
```

3. Replace with your actual MAC (convert from `:` format to hex `0x...`)

**Example:**

```cpp
// If MAC = A4:B1:C1:12:34:56
uint8_t broadcastAddress[] = {0xA4, 0xB1, 0xC1, 0x12, 0x34, 0x56};
```

---

### 🔨 STEP 3: Upload Code to Receiver ESP32

1. Open `RESS.ino` in Arduino IDE.
2. Check that the servo pins match the code:

```cpp
#define channel1Pin 14
#define channel2Pin 27
#define channel3Pin 26
```

3. Connect servos to the correct GPIO pins.
4. Use a separate 5V supply for servos and **common GND** with ESP32.
5. Upload code to Receiver ESP32.
6. Open Serial Monitor (baud: 115200) and look for:

```
ESPNow receiver initialized
Data received: Channel 1 = HIGH
```

---

### ✅ STEP 4: Final Testing – Make Sure Everything Works

1. **Power up both ESP32 boards** using USB or battery.
2. **Open Serial Monitors** for both if needed.

   * Transmitter should show:

     ```
     ```

Sending: 1 0 1

````
   - Receiver should show:
     ```
Data received: CH1 = 1, CH2 = 0, CH3 = 1
````

3. **Test your transmitter**:

   * Press buttons / Move joystick / Turn potentiometer
4. **Observe the receiver**:

   * Servos move / Relays or LEDs respond

#### Example Table:

| Transmitter Action   | Receiver Result           |
| -------------------- | ------------------------- |
| Press Button 1       | Servo 1 moves / CH1 ON    |
| Press Button 2       | Servo 2 moves / CH2 ON    |
| Move Joystick        | Servo angle adjusts       |
| Rotate Potentiometer | Speed or position changes |

---

### 🛠️ Troubleshooting

| Problem                 | Reason                   | Fix                          |
| ----------------------- | ------------------------ | ---------------------------- |
| No response on receiver | Wrong MAC address        | Check Step 2                 |
| Servos not working      | Weak power supply        | Use external 5V & common GND |
| Data not sending        | Wrong GPIO or code bug   | Check button pins & logic    |
| Upload fails            | Flash button not pressed | Hold BOOT while uploading    |

---

## 📅 Final Result

* Transmitter sends signals via ESP-NOW
* Receiver gets data and performs output
* You now have a **working 3-channel wireless controller** 🎉









