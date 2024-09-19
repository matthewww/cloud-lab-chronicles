# ESP32-CAM

Extend further - 
- Analytics
- Automated pipelines
- Dashboards for real-time monitoring

### Image Capture and Storage: AWS S3 / Azure Blob Storage
   **Objective**: Capture images from the ESP32-CAM and upload them to cloud storage for later analysis or viewing.

   **Steps**:
   - **ESP32-CAM**: Capture images periodically or based on a trigger (like motion detection).
   - **AWS**: Use the AWS SDK for Arduino to upload the captured images to an S3 bucket.
     - Use **AWS IoT Core** to securely transmit the image data to an AWS Lambda function, which then stores the image in an S3 bucket.
   - **Azure**: Use **Azure Blob Storage** to store the captured images.
     - Use **Azure IoT Hub** to send data from the ESP32-CAM, then use an Azure Function to save the images to Blob Storage.

   **Learning Outcomes**:
   - Understand how to integrate IoT devices with cloud storage.
   - Learn about secure data transmission and cloud storage using IoT services.

   **Required AWS Services**:
   - **S3** (Amazon Simple Storage Service) – Free Tier includes 5 GB storage.
   - **AWS IoT Core** – 250,000 messages per month.

   **Required Azure Services**:
   - **Azure Blob Storage** – Free Tier includes 5 GB of storage.
   - **Azure IoT Hub** – Free tier allows 8,000 messages per day.

---

### Motion Detection: AWS SNS / Azure Notification Hubs
   **Objective**: Set up motion detection (using basic image analysis or an external PIR sensor) with the ESP32-CAM and send notifications via email or SMS using AWS SNS (Simple Notification Service) or Azure Notification Hubs.

   **Steps**:
   - **ESP32-CAM**: Use the camera’s built-in motion detection (or use a PIR sensor) to trigger an event.
   - **AWS**: Send a message to **AWS IoT Core**, and trigger an **AWS Lambda** function that sends an email/SMS through **Amazon SNS**.
   - **Azure**: Use **Azure IoT Hub** to send a message to **Azure Functions**, which triggers **Azure Notification Hubs** to send a push notification or email.
   - **Possible Extensions**:
     - Include an AI-based object recognition feature to only send alerts for specific objects (e.g., people or vehicles).

   **Learning Outcomes**:
   - Explore the integration between edge IoT devices and cloud services.
   - Understand event-driven architectures in the cloud.

   **Required AWS Services**:
   - **AWS SNS** – Free tier includes 1 million notifications per month.
   - **AWS Lambda** – 1 million requests and 400,000 GB-seconds of compute time.

   **Required Azure Services**:
   - **Azure Notification Hubs** – Free tier allows 1 million push notifications.
   - **Azure IoT Hub** – Free tier with 8,000 messages per day.

---

### Live Streaming: AWS Kinesis Video Streams / Azure Media Services
   **Objective**: Stream live video from the ESP32-CAM to the cloud using AWS Kinesis Video Streams or Azure Media Services for real-time monitoring or video analytics.

   **Steps**:
   - **ESP32-CAM**: Set up the camera to capture and stream video.
   - **AWS**: Use **Kinesis Video Streams** to stream live video to the cloud. You can also use **Amazon Rekognition** for basic video analytics (e.g., object detection, face recognition).
   - **Azure**: Use **Azure Media Services** to stream the video and perform tasks like live video encoding, storage, and analysis.

   **Learning Outcomes**:
   - Understand how to transmit real-time video from an IoT device to the cloud.
   - Learn about video processing and analytics services in the cloud.

   **Required AWS Services**:
   - **Kinesis Video Streams** – Free Tier includes 1,000,000 PUT requests and 5 GB of storage.
   - **Amazon Rekognition** (Optional) – Free tier for video includes 60 minutes per month.

   **Required Azure Services**:
   - **Azure Media Services** – Free tier offers 20 encoding minutes and 5 GB of data transfer.

---

### Live Streaming: via MQTT
   - **Objective**: Stream live video from the ESP32-CAM using MQTT (AWS IoT Core or Azure IoT Hub) and display it on a dashboard.
   
   - **Tools Needed**:
     - ESP32-CAM
     - AWS IoT Core or Azure IoT Hub (free tier)
     - MQTT broker (AWS/Azure)
     - Dashboard service (AWS IoT Core, Azure IoT Hub, or custom app)

   - **Steps**:
     1. **ESP32-CAM**: Set up the camera to capture a continuous video stream.
     2. **MQTT**: Use MQTT to transmit images or video data to AWS IoT Core or Azure IoT Hub.
     3. **AWS/Azure**: Set up an AWS IoT or Azure IoT rule to forward the video stream to a dashboard.
     4. Display the video feed in real time on a dashboard or custom application.

   - **Learning Outcomes**:
     - Understand how to transmit real-time video from an IoT device to the cloud using MQTT.
     - Learn about setting up and using AWS IoT Core or Azure IoT Hub for video streaming.

   - **Required AWS Services**:
     - **AWS IoT Core** – Free tier includes 250,000 messages per month.
     - **AWS Lambda** (Optional) – Free tier includes 1,000,000 requests and 400,000 GB-seconds of compute time per month for processing video streams.

   - **Required Azure Services**:
     - **Azure IoT Hub** – Free tier includes 8,000 messages per day.
     - **Azure Functions** (Optional) – Free tier includes 1,000,000 requests and 400,000 GB-seconds of compute time per month for processing video streams.

   - **Possible Extensions**:
     - Integrate with AWS Lambda/Azure Functions to process the video stream in real-time for object detection or face recognition.

---

### Face Recognition: AWS Rekognition / Azure Cognitive Services
   **Objective**: Perform face recognition with the ESP32-CAM by integrating it with cloud-based AI services like AWS Rekognition or Azure Cognitive Services.

   **Steps**:
   - **ESP32-CAM**: Capture images and send them to the cloud for processing.
   - **AWS**: Use **AWS Rekognition** to detect and recognize faces from the images sent by the ESP32-CAM. You can also implement facial comparisons or label detection.
   - **Azure**: Use **Azure Cognitive Services** (specifically the **Face API**) to perform face detection and recognition on the images sent from the ESP32-CAM.

   - **Possible Extensions**:
     - Add a logging system where each recognized face is logged with a timestamp in AWS DynamoDB or Azure Cosmos DB.
     - Set up notifications when a specific face is recognized.
     - 
   **Learning Outcomes**:
   - Gain exposure to cloud-based AI and machine learning services.
   - Learn how to integrate computer vision with IoT devices.

   **Required AWS Services**:
   - **AWS Rekognition** – Free tier includes 5,000 images per month for face analysis.

   **Required Azure Services**:
   - **Azure Cognitive Services – Face API** – Free tier includes 30,000 transactions per month.

---

### Temperature Monitoring: AWS IoT Analytics / Azure Time Series Insights
   **Objective**: Use the ESP32-CAM along with a temperature sensor (e.g., DHT11 or DHT22) to monitor environmental conditions and visualize data in the cloud.

   **Steps**:
   - **ESP32-CAM**: Connect a DHT sensor to measure temperature and humidity. Send data to the cloud along with images captured periodically.
   - **AWS**: Use **AWS IoT Analytics** to analyze and visualize the temperature data. You can build time-series dashboards to monitor changes over time.
   - **Azure**: Use **Azure Time Series Insights** to create time-series graphs of temperature readings and explore the data interactively.
  
  **Possible Extensions**:
   - Set up temperature thresholds to trigger alerts or activate devices like fans.

   **Learning Outcomes**:
   - Understand how to collect and analyze time-series IoT data.
   - Learn how to build IoT data pipelines and visualize the results.

   **Required AWS Services**:
   - **AWS IoT Analytics** – Free Tier includes 2,500 messages per month.

   **Required Azure Services**:
   - **Azure Time Series Insights** – Free tier includes 30 days of data retention and 10,000 events.

---

### Security System: AWS IoT & Lambda / Azure Logic Apps
   **Objective**: Build a simple security system using the ESP32-CAM and cloud services to notify you whenever motion is detected, and store images for review.

   **Steps**:
   - **ESP32-CAM**: Use motion detection or a PIR sensor to trigger image capture.
   - **AWS**: Send the captured image to AWS IoT Core, then use **AWS Lambda** to store the image in S3 and notify you through **Amazon SNS**.
   - **Azure**: Use **Azure IoT Hub** to send data to **Azure Logic Apps**, where you can trigger storage of the image in Blob Storage and send a notification.

   **Learning Outcomes**:
   - Learn how to build a basic IoT security system.
   - Explore serverless functions to process IoT data and trigger events.

   **Required AWS Services**:
   - **AWS IoT Core** and **AWS Lambda** – Free tier as mentioned in previous examples.
   - **Amazon SNS** – Free tier includes 1 million notifications.

   **Required Azure Services**:
   - **Azure IoT Hub** and **Azure Logic Apps** – Free tier as mentioned in previous examples.

---

### Security Surveillance with AWS DynamoDB / Azure Cosmos DB
   - **Objective**: Use the ESP32-CAM to monitor a location and store image or video capture events in AWS DynamoDB or Azure Cosmos DB.
   
   - **Tools Needed**:
     - ESP32-CAM
     - AWS DynamoDB or Azure Cosmos DB (free tier)
     - AWS S3 or Azure Blob Storage (for storing images/videos)
     - Cloud-based dashboard service (AWS or Azure)

   - **Steps**:
     1. **ESP32-CAM**: Set up the camera to capture periodic images or detect motion.
     2. **Storage**: Store captured images or videos in AWS S3 or Azure Blob Storage.
     3. **Database**: Store metadata (like timestamps and camera ID) in DynamoDB or Cosmos DB, along with links to the captured images/videos.
     4. **Dashboard**: Use a cloud-based dashboard to query and visualize historical data (like images captured over time).

   - **Learning Outcomes**:
     - Understand how to capture and store surveillance data from an IoT device.
     - Learn about using cloud databases and storage services for managing surveillance data.
     - Develop skills in creating dashboards for visualizing historical data.

   - **Required AWS Services**:
     - **AWS DynamoDB** – Free tier includes 25 GB of storage.
     - **AWS S3** – Free tier includes 5 GB of standard storage.
     - **AWS CloudWatch** (Optional) – For monitoring and logging.

   - **Required Azure Services**:
     - **Azure Cosmos DB** – Free tier includes 400 RU/s provisioned throughput and 5 GB of storage.
     - **Azure Blob Storage** – Free tier includes 5 GB of standard storage.
     - **Azure Monitor** (Optional) – For monitoring and logging.

   - **Possible Extensions**:
     - Create a time-lapse video of all images captured over a period.
     - Integrate with AWS Lambda or Azure Functions to trigger alerts based on specific events (e.g., motion detected).

---

### Time-Lapse Video Creator with AWS S3 / Azure Blob Storage
   - **Objective**: Use the ESP32-CAM to capture images at regular intervals and upload them to AWS S3 or Azure Blob Storage to create a time-lapse video.
   
   - **Tools Needed**:
     - ESP32-CAM
     - AWS S3 or Azure Blob Storage (free tier)
     - Optional: AWS Lambda or Azure Functions for automation

   - **Steps**:
     1. **ESP32-CAM**: Set up the camera to capture images every X minutes or hours.
     2. **Storage**: Upload the captured images to AWS S3 or Azure Blob Storage.
     3. **Processing**: Use a script or service (AWS Lambda or Azure Functions) to stitch the images into a time-lapse video.
     4. **Output**: Store the generated time-lapse video in a separate bucket or location for easy access.

   - **Learning Outcomes**:
     - Understand how to capture and store images at regular intervals using an IoT device.
     - Learn about using cloud storage services for managing and storing images.
     - Develop skills in automating the creation of time-lapse videos using cloud functions.

   - **Required AWS Services**:
     - **AWS S3** – Free tier includes 5 GB of standard storage.
     - **AWS Lambda** (Optional) – Free tier includes 1,000,000 requests and 400,000 GB-seconds of compute time per month for processing images.

   - **Required Azure Services**:
     - **Azure Blob Storage** – Free tier includes 5 GB of standard storage.
     - **Azure Functions** (Optional) – Free tier includes 1,000,000 requests and 400,000 GB-seconds of compute time per month for processing images.

   - **Possible Extensions**:
     - Automate the video generation process, storing videos in a separate bucket or location for easy access.
     - Integrate with notification services (AWS SNS or Azure Notification Hubs) to alert when a new time-lapse video is available.

