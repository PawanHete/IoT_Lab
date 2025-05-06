
## Experiment Title: Basics of Network Protocols (CoAP, MQTT) and Cloud Integration (ThingSpeak)

### Aim:
Basic understanding of IoT networking protocols (CoAP, MQTT) and introduction to cloud integration using ThingSpeak.

### Materials Required:
- Raspberry Pi 5 Model B  
- Internet connection (Ethernet or Wi-Fi)  
- MQTT Broker (e.g., Mosquitto)  
- CoAP client/server tools (e.g., libcoap, Copper plugin for Firefox)  
- ThingSpeak account  
- Python 3 with the following libraries installed:
  - `paho-mqtt`
  - `requests`

### Circuit Diagram:
No physical circuit is required. This experiment is software-based and focuses on networking.

### Code:
```python
# MQTT Publisher Example using paho-mqtt

import paho.mqtt.client as mqtt
import time

broker = "test.mosquitto.org"
topic = "iot/lab/demo"

client = mqtt.Client("Publisher1")
client.connect(broker)

for i in range(5):
    message = f"Hello MQTT {i}"
    client.publish(topic, message)
    print(f"Sent: {message}")
    time.sleep(2)

client.disconnect()
```

```python
# ThingSpeak Data Upload Example

import requests
import time

API_KEY = "YOUR_THINGSPEAK_WRITE_API_KEY"
url = f"https://api.thingspeak.com/update?api_key={API_KEY}"

for i in range(5):
    response = requests.get(url + f"&field1={i}")
    print(f"Sent data to ThingSpeak: {i}, Response code: {response.status_code}")
    time.sleep(15)  # ThingSpeak allows an update every 15 seconds
```

### Process:

#### 1. **MQTT Basics**:
- Install the MQTT client library:
  ```bash
  pip install paho-mqtt
  ```
- Use the example script above to publish messages to a public broker.
- You can monitor these messages using an MQTT subscriber on another terminal or device.

#### 2. **CoAP Basics** (optional):
- CoAP (Constrained Application Protocol) can be explored using tools like:
  - `libcoap` CLI tools
  - Copper CoAP browser plugin (for Firefox)
- Example usage (if `libcoap` is installed):
  ```bash
  coap-client -m get coap://coap.me/test
  ```

#### 3. **ThingSpeak Integration**:
- Create a ThingSpeak channel at [https://thingspeak.com](https://thingspeak.com)
- Note your **Write API Key**
- Use the Python script provided to send data from your Raspberry Pi to the cloud.

### Observations and Results:
- MQTT messages are successfully published to the broker and can be viewed using an MQTT subscriber.
- ThingSpeak channel updates with the provided data every 15 seconds.
- CoAP GET request returns data if the client is correctly configured.

### Conclusion:
This experiment introduces key IoT communication protocols (MQTT, CoAP) and cloud integration using ThingSpeak. It highlights how Raspberry Pi can act as a networked IoT node capable of sending data to brokers or cloud platforms.  
**Extension Idea**: Connect a sensor to the Raspberry Pi and send real-time sensor data to ThingSpeak via MQTT or HTTP.
