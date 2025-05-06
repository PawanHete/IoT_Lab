
## Experiment Title: Introduction to Raspberry Pi 5 Model B and Its Components

### Aim:
Introduction to Raspberry Pi 5 Model B and its components

### Materials Required:
- Raspberry Pi 5 Model B  
- USB-C Power Supply (5V, 5A recommended)  
- microSD Card (32 GB or higher, with Raspberry Pi OS installed)  
- HDMI Monitor and micro-HDMI to HDMI cable  
- USB Keyboard and Mouse  
- Internet Connection (Ethernet or Wi-Fi)  
- Optional: Breadboard, LEDs, Resistors, Jumper Wires  

### Circuit Diagram:
This is an introductory experiment focused on getting familiar with Raspberry Pi 5, so no circuit diagram is required.  
However, for future hardware-based experiments, you can create circuit diagrams using tools like:
- [Fritzing](https://fritzing.org/)
- [Tinkercad Circuits](https://www.tinkercad.com/)
- [Lucidchart](https://www.lucidchart.com/)

### Code:
```python
# Optional GPIO Test - Blink an LED on GPIO 17
# Save this file as blink.py and run it on the Raspberry Pi after connecting an LED to GPIO 17.

import RPi.GPIO as GPIO
import time

LED_PIN = 17

GPIO.setmode(GPIO.BCM)
GPIO.setup(LED_PIN, GPIO.OUT)

print("Blinking LED. Press Ctrl+C to stop.")
try:
    while True:
        GPIO.output(LED_PIN, GPIO.HIGH)
        time.sleep(1)
        GPIO.output(LED_PIN, GPIO.LOW)
        time.sleep(1)
except KeyboardInterrupt:
    print("Stopped by User")
finally:
    GPIO.cleanup()
```

### Process:

1. **Hardware Setup**:
   - Insert the microSD card (with Raspberry Pi OS) into the Raspberry Pi.
   - Connect a monitor using a micro-HDMI to HDMI cable.
   - Plug in the USB keyboard and mouse.
   - Connect to the internet via Wi-Fi or Ethernet.
   - Power the Raspberry Pi using the USB-C power supply.

2. **First Boot Configuration**:
   - On the first boot, follow the on-screen setup wizard:
     - Select language and location.
     - Set up Wi-Fi.
     - Check for OS updates.
     - Create a user and password.

3. **Explore the Raspberry Pi 5 Components**:
   - USB 3.0 and USB 2.0 ports  
   - Dual micro-HDMI ports  
   - 40-pin GPIO header  
   - Ethernet port  
   - Camera and display ports (CSI, DSI)  
   - USB-C power input  
   - Onboard fan connector (for optional active cooling)  

4. **GPIO Test (Optional)**:
   - Connect an LED with a 330Ω resistor to GPIO 17 and GND.
   - Open a terminal and run the test script:
     ```bash
     python3 blink.py
     ```
   - Observe the LED blinking.

### Observations and Results:
- Raspberry Pi boots successfully into the desktop environment.
- All peripherals (mouse, keyboard, display) work correctly.
- If the LED is connected and the script is run, it blinks ON and OFF every second.

### Conclusion:
This experiment helps you get started with Raspberry Pi 5 Model B, including the first boot and familiarization with its hardware. It lays the groundwork for future IoT experiments involving sensors, actuators, and GPIO programming.  
**Extension Idea**: Try connecting and reading values from a DHT11 temperature and humidity sensor.
