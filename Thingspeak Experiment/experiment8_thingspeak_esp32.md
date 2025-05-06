# Experiment Title: Upload Temperature and Humidity Data to ThingSpeak Cloud using ESP32

## Aim:
Use an ESP32 microcontroller to read temperature and humidity data from a DHT11/DHT22 sensor and upload the readings to the ThingSpeak cloud platform in real time.

## Materials Required:

- ESP32 development board
- DHT11 or DHT22 temperature and humidity sensor
- Jumper wires
- Breadboard
- USB cable
- Arduino IDE
- ThingSpeak account
- Wi-Fi connection
- Libraries:
  - `DHT sensor library` by Adafruit
  - `Adafruit Unified Sensor` library
  - `WiFi.h`
  - `HTTPClient.h`

## Circuit Diagram:

- **DHT Sensor to ESP32 Connections:**
  - VCC → 3.3V
  - GND → GND
  - DATA → GPIO 4 (can be changed in code)

Use [Fritzing](https://fritzing.org/) to visually create and share circuit diagrams.

## Code:

```cpp
#include <WiFi.h>
#include <HTTPClient.h>
#include "DHT.h"

#define DHTPIN 4         // GPIO pin where the DHT sensor is connected
#define DHTTYPE DHT22    // Change to DHT11 if using that model

DHT dht(DHTPIN, DHTTYPE);

const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";
const char* apiKey = "YOUR_THINGSPEAK_API_KEY";
const char* server = "http://api.thingspeak.com/update";

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  dht.begin();

  Serial.print("Connecting to WiFi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("Connected!");
}

void loop() {
  float h = dht.readHumidity();
  float t = dht.readTemperature();

  if (isnan(h) || isnan(t)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }

  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;
    String url = String(server) + "?api_key=" + apiKey + "&field1=" + String(t) + "&field2=" + String(h);
    
    http.begin(url);
    int httpResponseCode = http.GET();
    http.end();

    Serial.print("Temperature: ");
    Serial.print(t);
    Serial.print(" °C, Humidity: ");
    Serial.print(h);
    Serial.println(" %");

    Serial.print("Response code: ");
    Serial.println(httpResponseCode);
  } else {
    Serial.println("WiFi Disconnected");
  }

  delay(15000);  // Upload every 15 seconds (ThingSpeak limit)
}
```

## Process:

1. **Create a ThingSpeak account and channel:**
   - Go to [https://thingspeak.com](https://thingspeak.com) and create a free account.
   - Create a new channel with two fields: Temperature and Humidity.
   - Note the Write API Key.

2. **Set up Arduino IDE:**
   - Install ESP32 board definitions via the Boards Manager.
   - Install required libraries: DHT, Adafruit Unified Sensor.
   - Select the correct board (e.g., "ESP32 Dev Module") and port.

3. **Upload the code:**
   - Replace `YOUR_WIFI_SSID`, `YOUR_WIFI_PASSWORD`, and `YOUR_THINGSPEAK_API_KEY` in the code.
   - Connect ESP32 to your PC and upload the code.

4. **Monitor results:**
   - Open Serial Monitor to view sensor readings and HTTP responses.
   - Go to your ThingSpeak channel to see real-time updates.

## Observations and Results:

- The Serial Monitor will display temperature and humidity readings every 15 seconds.
- The ThingSpeak channel dashboard will plot these values in real time.

## Conclusion:

This experiment demonstrates how to connect an ESP32 to a cloud IoT platform, read sensor data, and upload it periodically. This forms the basis for many real-world monitoring systems. You can extend this by adding more sensors or setting up alerts in ThingSpeak.

