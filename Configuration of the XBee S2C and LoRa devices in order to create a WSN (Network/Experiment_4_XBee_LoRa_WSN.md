
---

**Experiment Title:** Configuration of XBee S2C and LoRa Devices to Create a Wireless Sensor Network (WSN)

**Aim:**  
Introduction to configuring XBee S2C and LoRa modules for establishing a Wireless Sensor Network (WSN) for IoT applications.

---

**Materials Required:**

- 2 × XBee S2C modules (1 Coordinator, 1 Router/End Device)
- 2 × XBee USB adapters or breakout boards
- 2 × Computers (or a single PC with two USB ports)
- XCTU Software (by Digi International)
- 2 × LoRa SX1278 or similar modules
- Arduino Uno × 2
- Jumper wires, breadboard
- Arduino IDE
- USB cables

---

**Circuit Diagram:**

> *XBee:*
- Connect the XBee module to the USB adapter and plug it into your PC.
- Use **Digi’s XCTU software** to visualize network topologies and configure the modules.

> *LoRa:*
Use [Fritzing](https://fritzing.org/) or a similar tool to draw a basic wiring diagram:
- **LoRa SX1278 to Arduino Uno**
  - VCC → 3.3V
  - GND → GND
  - MISO → D12
  - MOSI → D11
  - SCK → D13
  - NSS/CS → D10
  - RESET → D9
  - DIO0 → D2

---

**Code:**

*LoRa Sender (Node 1)*

```cpp
#include <SPI.h>
#include <LoRa.h>

void setup() {
  Serial.begin(9600);
  while (!Serial);

  if (!LoRa.begin(433E6)) {
    Serial.println("LoRa init failed. Check connections.");
    while (1);
  }
  Serial.println("LoRa Sender Initialized.");
}

void loop() {
  Serial.println("Sending data...");
  LoRa.beginPacket();
  LoRa.print("Sensor Reading: ");
  LoRa.print(random(10, 100)); // Simulated data
  LoRa.endPacket();
  delay(2000);
}
```

*LoRa Receiver (Node 2)*

```cpp
#include <SPI.h>
#include <LoRa.h>

void setup() {
  Serial.begin(9600);
  while (!Serial);

  if (!LoRa.begin(433E6)) {
    Serial.println("LoRa init failed. Check connections.");
    while (1);
  }
  Serial.println("LoRa Receiver Initialized.");
}

void loop() {
  int packetSize = LoRa.parsePacket();
  if (packetSize) {
    Serial.print("Received packet: ");
    while (LoRa.available()) {
      Serial.print((char)LoRa.read());
    }
    Serial.println();
  }
}
```

---

**Process:**

### 🛰 XBee Configuration

1. Connect both XBee modules to your PC using USB adapters.
2. Launch **XCTU** software.
3. Detect the modules and update firmware if necessary.
4. Configure one module as **Coordinator**:
   - Set PAN ID (e.g., `1234`)
   - Set `CE = 1`
   - Set destination address to the other module
5. Configure the second module as **Router/End Device**:
   - Same PAN ID
   - `CE = 0`
   - Destination address set to Coordinator
6. Test communication using the **Console tab** in XCTU.

### 📡 LoRa Setup

1. Wire each LoRa module to an Arduino as per the circuit diagram.
2. Upload **Sender Code** to one Arduino.
3. Upload **Receiver Code** to the second Arduino.
4. Open Serial Monitor (9600 baud) on each Arduino to observe data transfer.

---

**Observations and Results:**

- For **XBee**, communication is verified if text sent from one module appears in the Console of the other in XCTU.
- For **LoRa**, the receiver serial monitor will display messages like:
  ```
  Received packet: Sensor Reading: 65
  ```
- Data is transmitted wirelessly using each protocol at low power and long range (especially with LoRa).

---

**Conclusion:**

This experiment demonstrated how to set up and configure **XBee S2C** modules using XCTU and how to implement a basic **LoRa-based WSN** using Arduino. It highlighted how low-power wireless communication can be established for sensor networks. This forms the backbone for many IoT applications like smart agriculture, environmental monitoring, and home automation.

**Extensions:**
- Add sensors (e.g., temperature) to transmit actual sensor readings.
- Use multiple nodes to form a mesh network.
- Integrate cloud-based data logging.

---
