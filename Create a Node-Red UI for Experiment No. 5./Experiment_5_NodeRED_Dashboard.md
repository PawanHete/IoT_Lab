
---

**Experiment Title:** Node-RED Dashboard for MQTT-based Real-Time IoT Communication

**Aim:**  
To create a Node-RED user interface (UI) dashboard that visualizes MQTT messages published by IoT devices in real-time using the Mosquitto broker.

---

**Materials Required:**

- Raspberry Pi or PC (with Node-RED installed)
- ESP8266/ESP32 with publisher code (from Experiment 5)
- Mosquitto MQTT broker
- Web browser (for UI access)
- Wi-Fi network

---

**Process:**

### 🛠 Node-RED Setup

1. **Install Node-RED** (if not already installed):
   ```bash
   sudo apt update
   sudo npm install -g --unsafe-perm node-red
   ```

2. **Install Node-RED Dashboard:**
   Open Node-RED editor at `http://localhost:1880`, then in the top right menu → "Manage palette" → Install:
   - Search for `node-red-dashboard` and install it.

3. **Import this Node-RED flow:**

```json
[
    {
        "id": "mqtt-in",
        "type": "mqtt in",
        "z": "flow1",
        "name": "Temperature Subscriber",
        "topic": "iot/sensor/temperature",
        "qos": "0",
        "broker": "mqtt-broker",
        "x": 160,
        "y": 100,
        "wires": [["ui-text"]]
    },
    {
        "id": "ui-text",
        "type": "ui_text",
        "z": "flow1",
        "group": "dashboard-group",
        "order": 1,
        "width": 6,
        "height": 1,
        "name": "Temperature Display",
        "label": "Temperature",
        "format": "{{msg.payload}}",
        "layout": "row-spread",
        "x": 400,
        "y": 100,
        "wires": []
    },
    {
        "id": "mqtt-broker",
        "type": "mqtt-broker",
        "name": "Local Mosquitto",
        "broker": "localhost",
        "port": "1883",
        "clientid": "",
        "usetls": false,
        "compatmode": false,
        "keepalive": "60",
        "cleansession": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthPayload": "",
        "closeTopic": "",
        "closePayload": "",
        "willTopic": "",
        "willQos": "0",
        "willPayload": ""
    },
    {
        "id": "dashboard-group",
        "type": "ui_group",
        "name": "Sensor Data",
        "tab": "dashboard-tab",
        "order": 1,
        "disp": true,
        "width": "6"
    },
    {
        "id": "dashboard-tab",
        "type": "ui_tab",
        "name": "MQTT Dashboard",
        "icon": "dashboard",
        "order": 1
    }
]
```

4. **Deploy the Flow:** Click "Deploy" to run the Node-RED flow.

5. **Access the UI Dashboard:**  
   Navigate to `http://localhost:1880/ui` to see the real-time temperature updates.

---

**Observations and Results:**

- The dashboard displays real-time temperature messages received from the MQTT topic `iot/sensor/temperature`.
- Updates occur every few seconds as the publisher sends new data.

---

**Conclusion:**

Node-RED's dashboard capabilities provide a simple and effective way to visualize MQTT-based IoT data. This low-code tool helps create interactive UIs suitable for prototyping and live monitoring in applications such as smart homes and industrial telemetry.

**Extensions:**
- Add charts or gauges for visualizing data trends.
- Use additional topics for humidity, pressure, etc.
- Add alert conditions and email/SMS notifications.

---
