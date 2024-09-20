# ESP32 and Components

### IoT Dashboard with ESP32 and AWS IoT or Azure IoT Hub
   **Objective**: Build a basic IoT dashboard to display sensor data on your OLED and stream it to the cloud using AWS IoT Core or Azure IoT Hub.

   **Components**:
   - ESP32 dev board
   - 128x64 OLED
   - Distance sensor (e.g., HC-SR04)
   - AWS IoT Core or Azure IoT Hub

   **Steps**:
   - **ESP32**: Connect the distance sensor to the ESP32 and read the data.
   - **OLED**: Display the sensor readings (distance) on the OLED screen.
   - **AWS**: Send the sensor data to **AWS IoT Core** using MQTT.
     - Use **AWS IoT Analytics** or **AWS CloudWatch** to visualize the data.
   - **Azure**: Send the data to **Azure IoT Hub** and visualize it with **Azure Time Series Insights** or a Power BI dashboard.
   - Display real-time distance readings on both the OLED and the cloud dashboard.

   **Learning Outcomes**:
   - Understand how to integrate sensors with IoT platforms.
   - Explore MQTT communication and cloud-based dashboards for IoT devices.

   **Required AWS Services**:
   - **AWS IoT Core** (Free tier: 250,000 messages per month).
   - **AWS IoT Analytics** (Free tier includes 2,500 messages per month).

   **Required Azure Services**:
   - **Azure IoT Hub** (Free tier: 8,000 messages per day).
   - **Azure Time Series Insights** (Free tier for visualizing sensor data).

---

### Gesture-Controlled Cloud Dashboard with Touchpad and ESP32
   **Objective**: Use the 9-button touchpad to interact with the cloud, triggering different actions based on touch inputs, and display feedback on the OLED.

   **Components**:
   - ESP32 dev board
   - 128x64 OLED
   - 9-button touchpad
   - AWS or Azure

   **Steps**:
   - **ESP32**: Connect the 9-button touchpad to the ESP32 and detect touch inputs.
   - **Cloud Integration**: Map touch events to different actions. For instance:
     - Button 1 might trigger sending a message to **AWS IoT Core** or **Azure IoT Hub**.
     - Button 2 might trigger a serverless function (AWS Lambda or Azure Functions).
   - **AWS**: Use **AWS IoT Core** to capture touch events, then trigger an **AWS Lambda** function to respond with an action (e.g., store data in DynamoDB or send a notification via SNS).
   - **Azure**: Use **Azure IoT Hub** to capture touch events and trigger **Azure Functions** to respond with actions.
   - **OLED Display**: Show which button was pressed and the cloud response on the OLED display.

   **Learning Outcomes**:
   - Understand how to map touchpad inputs to cloud actions.
   - Explore serverless functions and event-driven architectures.

   **Required AWS Services**:
   - **AWS IoT Core**.
   - **AWS Lambda** (1 million requests/month free tier).

   **Required Azure Services**:
   - **Azure IoT Hub**.
   - **Azure Functions** (1 million executions per month free tier).

---

### Cloud-Connected Environmental Monitor with Gyro/Accelerometer and Distance Sensor
   **Objective**: Build an environmental monitoring system that sends accelerometer/gyro data, as well as distance readings, to the cloud for real-time analysis.

   **Components**:
   - ESP32 dev board
   - 128x64 OLED
   - MPU6050 (Gyro/Accelerometer)
   - Distance sensor
   - AWS or Azure

   **Steps**:
   - **ESP32**: Read data from the MPU6050 and the distance sensor.
   - **OLED**: Display the current sensor readings on the OLED.
   - **AWS**: Send the sensor data to **AWS IoT Core** and use **AWS IoT Analytics** to visualize trends (e.g., movement patterns or distance changes).
   - **Azure**: Send the sensor data to **Azure IoT Hub** and visualize the data in **Azure Time Series Insights**.
   - Explore ways to detect specific conditions, such as sudden movement (based on accelerometer data), and trigger alerts or actions via the cloud.

   **Learning Outcomes**:
   - Explore IoT data collection from multiple sensors.
   - Understand cloud-based analysis and visualization.

   **Required AWS Services**:
   - **AWS IoT Core**.
   - **AWS IoT Analytics**.

   **Required Azure Services**:
   - **Azure IoT Hub**.
   - **Azure Time Series Insights**.

---

### Rotary Encoder as a Cloud-Controlled Volume Knob for a Cloud-Based Music Player
   **Objective**: Use the rotary encoder to adjust the volume of a cloud-based music player or trigger music tracks stored in cloud storage (AWS S3 or Azure Blob Storage).

   **Components**:
   - ESP32 dev board
   - Rotary encoder
   - PAM8403 amplifier
   - AWS or Azure

   **Steps**:
   - **ESP32**: Connect the rotary encoder to the ESP32 to act as a volume control.
   - **Cloud Integration**: Integrate with a cloud-based service to adjust volume or trigger audio playback based on rotary inputs.
     - **AWS**: Store audio files in **Amazon S3** and use **AWS Lambda** to play music when the rotary encoder is rotated.
     - **Azure**: Store music files in **Azure Blob Storage** and use **Azure Functions** to handle playback.
   - **Optional**: Integrate the **PAM8403 amplifier** to build a small speaker system, where the ESP32 controls the volume and music selection via cloud services.

   **Learning Outcomes**:
   - Explore cloud-based media storage and control using IoT devices.
   - Understand how to trigger serverless functions from IoT devices.

   **Required AWS Services**:
   - **Amazon S3** (Free tier includes 5 GB storage).
   - **AWS Lambda**.

   **Required Azure Services**:
   - **Azure Blob Storage** (Free tier includes 5 GB storage).
   - **Azure Functions**.

---

### Security System with Cloud Alerts Using ESP32 and Distance Sensor
   **Objective**: Create a simple security system where the distance sensor triggers an alert if an object comes too close. Alerts are sent via AWS SNS or Azure Notification Hubs.

   **Components**:
   - ESP32 dev board
   - Distance sensor (e.g., HC-SR04)
   - AWS or Azure

   **Steps**:
   - **ESP32**: Set up the distance sensor to monitor proximity.
   - **OLED**: Display the current distance readings on the OLED.
   - **Cloud Integration**: If the distance sensor detects an object within a certain range, trigger an alert:
     - **AWS**: Send a message to **AWS IoT Core**, which triggers an **AWS Lambda** function to send an email or SMS via **Amazon SNS**.
     - **Azure**: Use **Azure IoT Hub** to trigger an **Azure Logic App** or **Azure Notification Hubs** to send an email or push notification.
   - **Optional**: Store event logs (e.g., each detection event) in cloud storage (Amazon S3 or Azure Blob Storage).

   **Learning Outcomes**:
   - Understand how to integrate sensors with event-based cloud notification systems.
   - Explore cloud-based alerts and data storage.

   **Required AWS Services**:
   - **AWS IoT Core**.
   - **Amazon SNS** (Free tier: 1 million notifications/month).

   **Required Azure Services**:
   - **Azure IoT Hub**.
   - **Azure Notification Hubs** (Free tier: 1 million notifications/month).

---

### Gesture-Based Home Automation with Touchpad and Cloud
   **Objective**: Use the 9-button touchpad to control a cloud-based home automation system, allowing users to control devices such as lights or fans via AWS IoT or Azure IoT Hub.

   **Components**:
   - ESP32 dev board
   - 9-button touchpad
   - AWS or Azure

   **Steps**:
   - **ESP32**: Connect the touchpad to the ESP32 and detect touch gestures.
   - **Cloud Integration**: Map touchpad buttons to different actions. For example:
     - Button 1 turns on a cloud-connected light via **AWS IoT Core** or **Azure IoT Hub**.
     - Button 2 turns off the light.
   - Use **AWS Lambda** or **Azure Functions** to process commands and send control signals to the devices.
   - Display the current state of the devices (e.g., lights on/off) on the OLED.

   **Learning Outcomes**:
   - Learn how to create a cloud-connected home automation system.
   - Explore MQTT communication for remote control of devices.

   **Required AWS Services**:
   - **AWS IoT Core**.
   - **AWS Lambda**.

   **Required Azure Services**:
   - **Azure IoT Hub**.
   - **Azure Functions**.
