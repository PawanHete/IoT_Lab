
---

**Experiment Title:** Real-time Application of MQTT Protocol using Mosquitto Broker

**Aim:**  
Apply the MQTT protocol using the Mosquitto broker in a real-time IoT communication scenario.

---

**Materials Required:**

- Raspberry Pi or PC (as MQTT broker)
- ESP8266/ESP32 development board × 2 (or any MQTT-compatible microcontrollers)
- Arduino IDE
- Wi-Fi network with internet access
- Mosquitto MQTT broker (installed on Raspberry Pi or PC)
- MQTT client tool (e.g., MQTT.fx or MQTT Explorer)
- USB cables, jumper wires

---

**Circuit Diagram:**

Use [Fritzing](https://fritzing.org/) or similar tool to show ESP8266/ESP32 connected to power via USB and sensors (optional). Basic setup assumes onboard capabilities of ESP modules.

---

**Code:**

*ESP8266 MQTT Publisher (e.g., sending temperature data)*

```cpp
#include <ESP8266WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Your_SSID";
const char* password = "Your_PASSWORD";
const char* mqtt_server = "192.168.1.100"; // Replace with broker IP

WiFiClient espClient;
PubSubClient client(espClient);

void setup_wifi() {
  delay(10);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
  }
}

void setup() {
  Serial.begin(9600);
  setup_wifi();
  client.setServer(mqtt_server, 1883);
}

void loop() {
  if (!client.connected()) {
    while (!client.connected()) {
      client.connect("ESP8266Client");
    }
  }
  client.loop();

  float temperature = random(20, 30); // Simulated temperature
  char msg[50];
  sprintf(msg, "Temperature: %.2f", temperature);
  client.publish("iot/sensor/temperature", msg);
  delay(3000);
}
```

*ESP8266 MQTT Subscriber (e.g., displaying messages)*

```cpp
#include <ESP8266WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Your_SSID";
const char* password = "Your_PASSWORD";
const char* mqtt_server = "192.168.1.100"; // Replace with broker IP

WiFiClient espClient;
PubSubClient client(espClient);

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("]: ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();
}

void setup_wifi() {
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
  }
}

void setup() {
  Serial.begin(9600);
  setup_wifi();
  client.setServer(mqtt_server, 1883);
  client.setCallback(callback);
  while (!client.connected()) {
    client.connect("ESP8266ClientSubscriber");
  }
  client.subscribe("iot/sensor/temperature");
}

void loop() {
  client.loop();
}
```

---

**Process:**

1. Install Mosquitto broker on Raspberry Pi or PC:
   ```bash
   sudo apt update
   sudo apt install mosquitto mosquitto-clients
   sudo systemctl enable mosquitto
   ```
2. Connect ESP devices to the same network as the broker.
3. Upload publisher code to one ESP8266 and subscriber code to another.
4. Open Serial Monitor to observe publishing and receiving of messages.
5. Use MQTT.fx or `mosquitto_sub` and `mosquitto_pub` to test topics manually.

---

**Observations and Results:**

- Publisher sends messages like `Temperature: 25.34` every few seconds.
- Subscriber receives and displays these messages instantly.
- Broker (Mosquitto) routes data between devices using topics.

---

**Conclusion:**

This experiment demonstrated real-time communication between IoT devices using the MQTT protocol and Mosquitto broker. It showcased lightweight, efficient, and scalable message delivery suited for IoT. Such systems can be used in smart homes, health monitoring, and industrial automation.

**Extensions:**
- Connect actual sensors for live data.
- Secure with TLS and authentication.
- Add dashboard visualization using Node-RED or Grafana.

---
