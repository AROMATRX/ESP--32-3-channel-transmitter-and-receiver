# ESP--32-3-channel-transmitter-and-receiver
🔧 ES-32 3-Channel Transmitter &amp; Receiver A simple and efficient 3-channel communication system using two ESP32 boards via ESP-NOW protocol. Ideal for wireless control of RC models, robots, Drones or IoT devices.
 
 A fully wireless 3-channel communication system using two ESP32 boards and the ESP-NOW protocol — ideal for remote control applications like RC cars, robots, and automation projects.

---

## 📦 Project Structure

## 📡 How ESP-NOW Works

ESP-NOW is a wireless protocol by Espressif that allows ESP32 boards to communicate without Wi-Fi or internet. It's fast and reliable, perfect for real-time transmission.

---

## 🧰 Hardware Required

| Component              | Quantity |
|------------------------|----------|
| ESP32 WROOM Dev Board  | 2        |
| Tactile Push Buttons   | 4        |
| Analog Joystick Module | 1        |
| Potentiometer (5kΩ)    | 1        |
| Micro Servo Motors (SG90) | 3     |
| Jumper Wires           | Many     |
| Breadboards (optional) | 2        |

---

## 📍 Wiring Diagrams

### 🟦 Transmitter Setup
![Transmitter Wiring](Transmitter.png)

- Buttons:
  - LEFT/RIGHT = GPIO 21 / GPIO 22
  - THROTTLE/STEERING = GPIO 18 / GPIO 19
- Joystick:
  - X-Axis = GPIO 34
  - Y-Axis = GPIO 35
- Potentiometer = GPIO 32

---

### 🟥 Receiver Setup
![Receiver Wiring](Receiver.png)

- Servo Channels:
  - CH1 (e.g., Steering) = GPIO 14
  - CH2 (e.g., Throttle) = GPIO 27
  - CH3 (e.g., Auxiliary) = GPIO 26

> ⚠️ **Note:** Do not power the servos directly from the ESP32 3.3V. Use external 5V supply.or if your esp have an option for 5v in your esp32 then you can take as a power supply.

---

## 🔄 Step-by-Step Setup

### 🧪 STEP 1: Get the Receiver MAC Address

1. Open `Get_MAC_Address.ino` in Arduino IDE  
2. Select correct COM port and ESP32 board  
3. Upload and open **Serial Monitor**  
4. Copy the MAC address (e.g. `24:6F:28:AB:CD:EF`)

```cpp
void setup() {
  Serial.begin(115200);
  WiFi.mode(WIFI_STA);
  Serial.println(WiFi.macAddress());
}


🔧 STEP 2: How to Add Receiver MAC Address to Transmitter Code
To send data from one ESP32 (Transmitter) to another ESP32 (Receiver), you need the MAC address of the receiver.

Here’s how to do it:

✅ 1. Get the MAC Address of Receiver
Open the file Get_MAC_Address.ino in Arduino IDE.

Connect your Receiver ESP32 to your computer.

Select the correct board (ESP32 Dev Module) and COM port.

Click the Upload button.

Open Serial Monitor (top right corner of Arduino IDE).

You will see something like this:

 Mac-address 24:6F:28:AB:CD:EF..This is the MAC address of your Receiver.

🛠 2. Add This MAC Address to Transmitter Code
Open the file transmm.ino in Arduino IDE.

Find this line in the code:
uint8_t broadcastAddress[] = {0x24, 0x6F, 0x28, 0xAB, 0xCD, 0xEF};

Change the numbers to match the MAC address you got from the Serial Monitor.

🔸 Note: Each part of the MAC address must start with 0x and be separated by commas.

Example:

If MAC is A4:B1:C1:12:34:56, then write:
uint8_t broadcastAddress[] = {0xA4, 0xB1, 0xC1, 0x12, 0x34, 0x56};

🔧 STEP 3: Upload Code to Receiver ESP32.
Once the transmitter is ready and MAC address is added, now we will set up the Receiver ESP32. This device will receive the data and control servos or other outputs.

🧪 1. Open the Receiver Code
Open the file RESS.ino in Arduino IDE.

This is the code for the Receiver ESP32.

It receives wireless data from the Transmitter using ESP-NOW.

⚙️ 2. Check Pin Connections
Make sure the servo motors or output devices are connected to the correct GPIO pins.

📌 Default Pin Setup in the Code:
Channel	GPIO Pin
CH1	GPIO 14
CH2	GPIO 27
CH3	GPIO 26

If you want to change pins, update them in the code:

#define channel1Pin 14
#define channel2Pin 27
#define channel3Pin 26
🔌 3. Wiring Guide (with power tip)
Connect servo signal wires to GPIO 14, 27, 26.

Use a separate 5V power source for servos (not from ESP32 3.3V).

Common GND is important: connect ESP32 GND and Servo GND together.

✅ Refer to the Receiver.png diagram for the full wiring.

📤 4. Upload the Code
Plug your Receiver ESP32 into the computer.

Select:

Board: ESP32 Dev Module

Port: Correct COM Port

Click the Upload button.

After upload, open Serial Monitor (9600 or 115200 baud).

You should see messages like:



ESPNow receiver initialized
Data received: Channel 1 = HIGH
✅ Final Check
Now your receiver is ready!

When you press buttons or move joystick on Transmitter, data is sent wirelessly.

Receiver reads it and moves servos or switches outputs.

✅ STEP 4: Final Testing – Check if Everything is Working
After uploading the code to both Transmitter and Receiver, it's time to test your setup and make sure everything works properly.

📡 1. Power Up Both ESP32 Boards
Plug in both ESP32 boards (Transmitter & Receiver) using USB cables or external 5V sources (like battery or adapter).

Make sure both are powered at the same time.

🔍 2. Open Serial Monitors (Optional but Helpful)
On Transmitter: Open Serial Monitor to see if data is being sent.

You should see something like:

makefile
Copy
Edit
Sending: 1 0 1
On Receiver: Open Serial Monitor to see if data is being received.

Output example:

yaml
Copy
Edit
Data received: CH1 = 1, CH2 = 0, CH3 = 1
📌 Note: Make sure both Serial Monitors are set to the correct baud rate (usually 115200).

🎮 3. Test Your Transmitter Controls
Press Buttons / Use Joystick
Depending on your transmitter setup:

Press Button 1, Button 2, Button 3

Move joystick if you're using analog input

Rotate potentiometer if connected

Check if your Transmitter sends changing values to the Receiver.

⚙️ 4. Observe Receiver Outputs
At the same time:

Servo motors should move according to the received signals.

If you're using LEDs or relays, they should turn ON/OFF based on input.

You can also see the values on Serial Monitor.

🧪 Example Test Table:
Action on Transmitter	What Should Happen on Receiver
Press Button 1	Servo 1 moves / CH1 turns ON
Press Button 2	Servo 2 moves / CH2 turns ON
Move Joystick	Servo angle changes smoothly
Turn Potentiometer	Speed or angle adjusts

🛠 Troubleshooting
Problem	Reason	Fix
No response on receiver	Wrong MAC address	Re-check Step 2
Servos not working	Low power	Use 5V power with common GND
Transmitter not sending data	Code error or wrong wiring	Double-check button pins
Receiver not receiving	ESP-NOW init failed or not paired	Check esp_now_add_peer() return
Upload fails	Press BOOT while uploading (ESP32)	Try again

✅ Final Result
If everything is working:

Transmitter is sending signals

Receiver is receiving and acting (servo/LED/relay)

You have a fully working 3-channel wireless system 🎉...















