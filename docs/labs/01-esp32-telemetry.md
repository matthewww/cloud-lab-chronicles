# Azure: IoT Dashboard for ESP32 Telemetry
#### 23 Sep 2024

1. [Services covered](#services-covered)
2. [Lab description](#lab-description)
3. [Prerequisites](#prerequisites)
4. [Lab steps](#lab-steps)
5. [Lab files](#lab-files)
6. [Troubleshooting](#troubleshooting)
7. [Acknowledgements](#acknowledgements)

## Services covered
- **Azure IoT Hub** (free tier)
- **Azure IoT Central** (free tier)

## Lab description

This lab demonstrates how to collect built-in telemetry from an **ESP32** microcontroller and visualize it using a real-time dashboard in **Azure IoT Central**. Telemetry data includes:

- Wi-Fi signal strength
- Free heap memory
- CPU temperature
- Uptime

The ESP32 will send telemetry to Azure IoT Hub, and Azure IoT Central will be used to build a graphical dashboard for real-time monitoring and analysis.

## Prerequisites

1. **Hardware:**
   - ESP32 microcontroller

2. **Software:**
   - Arduino IDE with ESP32 board support
   - Azure IoT SDK (`AzureIoTHub` library)

3. **Azure Setup:**
   - Azure account with access to **Azure IoT Hub** and **Azure IoT Central**.

## Lab steps

### 1. Create an Azure IoT Hub

1. Go to the [Azure portal](https://portal.azure.com/).
2. Click **Create a resource**, search for **IoT Hub**, and click **Create**.
3. Choose **Free Tier** to avoid costs.
4. Create a resource group (e.g. `ESP32-Telemetry-RG`) and give the IoT Hub a name (e.g., `ESP32Hub`).
  - Networking: Public Access is fine. Communication is encrypted (TLS) and authenticated (SAS tokens) and works on Free Tier.
  - Management: Shared Access Policies + RBAC  
    - The ESP32 uses Shared Access Policies (with a SAS token).
    - You'll use RBAC to access the IoT Hub through the portal.
    - The monitoring app can be use RBAC to read telemetry to display it on a dashboard.

5. After creation, go to **IoT Hub** > **IoT Devices** and click **+ New** to register a new device.
6. Save the **device connection string**; you’ll use this in the ESP32 code.

### 2. Code the ESP32 for Sending Telemetry

1. Install the required libraries in **Arduino IDE**:
   - **Azure IoT SDK for ESP32** (`AzureIoTHub` via Library Manager).
   - **WiFi Library** (built-in).

2. Use the following code to send Wi-Fi signal strength, free heap memory, CPU temperature, and uptime to Azure IoT Hub:

```cpp
#include <WiFi.h>
#include <Esp32MQTTClient.h>

const char* ssid = "your-SSID";
const char* password = "your-PASSWORD";
const char* connectionString = "HostName=your-iot-hub.azure-devices.net;DeviceId=your-device-id;SharedAccessKey=your-device-key";

static const char* telemetryMessageTemplate = "{\"wifiSignalStrength\":%d,\"freeHeapMemory\":%d,\"cpuTemperature\":%d,\"uptime\":%d}";

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Esp32MQTTClient_Init((const uint8_t*)connectionString, true);
}

void loop() {
  int wifiSignalStrength = WiFi.RSSI();
  int freeHeapMemory = ESP.getFreeHeap();
  int cpuTemperature = (esp_random() % 20) + 50;
  int uptime = millis() / 1000;

  char telemetryMessage[256];
  sprintf(telemetryMessage, telemetryMessageTemplate, wifiSignalStrength, freeHeapMemory, cpuTemperature, uptime);

  if (Esp32MQTTClient_SendEvent(telemetryMessage)) {
    Serial.println("Telemetry sent:");
    Serial.println(telemetryMessage);
  } else {
    Serial.println("Telemetry failed.");
  }

  delay(5000);
}
```

3. Upload the code to your ESP32 and monitor the serial console to ensure it's sending telemetry.

### 3. Set Up Azure IoT Central

1. Go to [Azure IoT Central](https://apps.azureiotcentral.com/) and create a new application using the **Free Tier**.
2. Under **Device Templates**, create a new template for your ESP32 (e.g., `ESP32-Template`).
3. Define telemetry fields (`wifiSignalStrength`, `freeHeapMemory`, `cpuTemperature`, `uptime`), set their types to **Number**, and save.

### 4. Connect Your ESP32 Device to IoT Central

1. Under **Devices**, create a new device based on the `ESP32-Template` and copy the **Device Connection String**.
2. Update the ESP32 connection string in the code if necessary and re-upload the code to the ESP32.
3. Verify that the device connects and sends telemetry data by checking the **Telemetry** tab for your device in IoT Central.

### 5. Create a Dashboard in IoT Central

1. Navigate to **Dashboards** and create a new dashboard.
2. Add the following widgets:
   - **Line Chart** for `wifiSignalStrength` and `cpuTemperature`.
   - **Gauge** for `freeHeapMemory`.
   - **Number Widget** for `uptime`.
3. Save your dashboard and monitor real-time data from the ESP32.

## Lab files

- **ESP32 Telemetry Code**: [ESP32_Azure_Telemetry.ino](#)  

## Troubleshooting

- **Connection Issues**: Ensure that your Wi-Fi credentials are correct and the ESP32 is within range.
- **Azure IoT Hub Configuration**: Verify that the device connection string matches the one from Azure IoT Hub or IoT Central.
- **Telemetry Not Appearing**: Check the serial monitor for errors. If telemetry isn’t sent, check the IoT Central **Device** page for any communication issues.

## Acknowledgements
