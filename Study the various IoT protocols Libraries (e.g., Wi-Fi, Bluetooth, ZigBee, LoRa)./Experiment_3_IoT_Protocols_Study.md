
## Experiment Title: Study of Various IoT Protocol Libraries (Wi-Fi, Bluetooth, ZigBee, LoRa)

### Aim:
To study and understand various IoT communication protocols such as Wi-Fi, Bluetooth, ZigBee, and LoRa, and explore relevant Python libraries for interfacing with them.

### Materials Required:
- Raspberry Pi 5 Model B  
- Wi-Fi adapter (if not built-in)  
- Bluetooth-enabled module or device  
- ZigBee modules (e.g., XBee with USB adapter)  
- LoRa modules (e.g., SX1278 with USB interface or SPI interface)  
- USB to Serial adapter (for ZigBee/LoRa)  
- Python libraries:
  - `socket` (Wi-Fi)
  - `pybluez` (Bluetooth)
  - `xbee` (ZigBee)
  - `pyLoRa` or `pySX127x` (LoRa)

### Circuit Diagram:
No specific circuit for this experiment. Modules should be connected via USB or GPIO/SPI headers depending on protocol.  
Use tools like [Fritzing](https://fritzing.org/) to visualize connections if required.

### Code:
```python
# Wi-Fi Communication Example (TCP Client)
import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(("example.com", 80))
client.sendall(b"GET / HTTP/1.1\r\nHost: example.com\r\n\r\n")
response = client.recv(4096)
print(response.decode())
client.close()
```

```python
# Bluetooth Scan Example using pybluez
import bluetooth

print("Searching for Bluetooth devices...")
devices = bluetooth.discover_devices(duration=8, lookup_names=True)

for addr, name in devices:
    print(f"Device: {name}, Address: {addr}")
```

```python
# ZigBee Read Example using digi-xbee (requires serial device)
from digi.xbee.devices import XBeeDevice

device = XBeeDevice("/dev/ttyUSB0", 9600)
device.open()
xbee_message = device.read_data()
if xbee_message:
    print(f"Received data: {xbee_message.data.decode()}")
device.close()
```

```python
# LoRa Example (Basic Send/Receive using pyLoRa)
from SX127x.LoRa import LoRa
from SX127x.board_config import BOARD

BOARD.setup()
lora = LoRa()
lora.set_mode(LoRa.MODE.STDBY)
lora.set_freq(433)

print("Sending message via LoRa...")
lora.write_payload([0x48, 0x65, 0x6C, 0x6C, 0x6F])  # "Hello"
lora.set_mode(LoRa.MODE.TX)
```

### Process:

1. **Wi-Fi**:
   - Use the built-in Wi-Fi interface of Raspberry Pi.
   - Run the socket example to establish a TCP connection to a server.

2. **Bluetooth**:
   - Install pybluez:
     ```bash
     sudo apt install bluetooth libbluetooth-dev
     pip install pybluez
     ```
   - Run the Bluetooth scanner to discover nearby devices.

3. **ZigBee**:
   - Connect ZigBee module to Raspberry Pi via USB/serial.
   - Install `digi-xbee` library:
     ```bash
     pip install digi-xbee
     ```
   - Run the ZigBee receiver script.

4. **LoRa**:
   - Connect SX127x-based LoRa module to Raspberry Pi via SPI.
   - Install pySX127x:
     ```bash
     git clone https://github.com/LoRaWanMiniHub/pySX127x.git
     cd pySX127x
     sudo python3 setup.py install
     ```
   - Run the LoRa sender or receiver script.

### Observations and Results:
- Wi-Fi client receives an HTTP response from a server.
- Bluetooth scan lists nearby discoverable devices.
- ZigBee module receives data if a paired module is transmitting.
- LoRa module sends data over long-range, low-power radio communication.

### Conclusion:
This experiment demonstrates how different IoT protocols work and how to interact with them programmatically using Python libraries. Each protocol has its unique strengths and use-cases in IoT scenarios.  
**Extension Idea**: Integrate multiple protocols in one system—for example, collect data via Bluetooth and forward it over Wi-Fi or LoRa.
