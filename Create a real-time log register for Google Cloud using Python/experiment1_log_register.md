# Experiment Title: Real-Time Log Register for Google Cloud using Python

## Aim:
Create a real-time log register for Google Cloud using Python. This experiment demonstrates how to collect, format, and send log data to Google Cloud Logging in real-time, suitable for monitoring IoT applications.

## Materials Required:

- Raspberry Pi 5 Model B (or any computer with internet access)
- Python 3.x installed
- Google Cloud Platform (GCP) account
- GCP project with Cloud Logging API enabled
- Google Cloud SDK installed and configured
- Python libraries:
  - `google-cloud-logging`
  - `datetime`
  - `time`

## Circuit Diagram:

*No hardware connections are required for this experiment.*

**Optional Tools for Diagram Creation (if sensor or hardware is included later):**
- [Fritzing](https://fritzing.org)
- [Tinkercad Circuits](https://www.tinkercad.com/circuits)

## Code:

```python
# real_time_logger.py

from google.cloud import logging
import time
from datetime import datetime

# Initialize Google Cloud Logging client
client = logging.Client()
logger = client.logger("iot-log-register")

def generate_log_entry():
    """Simulates log entry creation with timestamps."""
    current_time = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    return f"Sensor event at {current_time}"

def main():
    print("Starting real-time log register...")
    try:
        while True:
            log_entry = generate_log_entry()
            logger.log_text(log_entry)
            print(f"Logged: {log_entry}")
            time.sleep(5)  # Log every 5 seconds
    except KeyboardInterrupt:
        print("Logging stopped by user.")

if __name__ == "__main__":
    main()
```

## Process:

1. **Enable the Cloud Logging API:**
   - Visit the [GCP Console](https://console.cloud.google.com/).
   - Create or select a project.
   - Enable the **Cloud Logging API** from the APIs & Services dashboard.

2. **Set up authentication:**
   - Go to IAM & Admin → Service Accounts.
   - Create a new service account and generate a JSON key.
   - Save this JSON key securely and set the environment variable:
     ```bash
     export GOOGLE_APPLICATION_CREDENTIALS="path/to/your/service-account.json"
     ```

3. **Install the required Python libraries:**
   ```bash
   pip install google-cloud-logging
   ```

4. **Run the script:**
   ```bash
   python real_time_logger.py
   ```

5. **View logs:**
   - Go to **Logging > Logs Explorer** in GCP.
   - Filter by the `iot-log-register` logger name to view the logs.

## Observations and Results:

- Every 5 seconds, a new log entry with the current timestamp should be sent to Google Cloud.
- You can verify the logs from the Logs Explorer in GCP in real time.
- The `print()` statement will also display the same messages locally on the terminal.

## Conclusion:

This experiment introduced students to using the `google-cloud-logging` Python SDK to implement a real-time log register. It provides foundational experience in working with cloud APIs and setting up remote logging for IoT systems. As an extension, you could modify the script to include real sensor data or alert conditions.
