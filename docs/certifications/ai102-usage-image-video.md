# Azure AI Services — Images & Video Overview  
### An AI-102 Mental Map

---

## 🖼️ **Images**

### 1. **Azure AI Vision Overview**  
| **Aspect**            | **Details** |
|-----------------------|-------------|
| **Service Name**      | Azure AI Vision (Computer Vision API) |
| **Primary Use**       | General-purpose image and video analysis using pretrained models |
| **Capabilities**      | OCR, image description, tags, object detection, spatial analysis, face detection |
| **Auth/Region**       | Key + region-specific endpoint, e.g. `https://<region>.api.cognitive.microsoft.com/` |
| **SDK**               | `azure-cognitiveservices-vision-computervision` |
| **Common SDK Client** | `ComputerVisionClient` |
| **Common REST Endpoint** | `POST /vision/v3.2/analyze` |
| **Inputs**            | Image URL or stream; query parameters: `visualFeatures`, `details`, `language` |

---

### 1.1 **Image Analysis (Tags, Description, Objects, etc.)**

| **Feature**           | **Image Analysis** |
|-----------------------|--------------------|
| **Use Case**          | Extract high-level information about an image (e.g. what's in it, objects, categories) |
| **visualFeatures**    | `Description`, `Tags`, `Objects`, `Categories`, `Brands`, `Adult`, etc. |
**Sample Request** 
```http
POST /vision/v3.2/analyze?visualFeatures=Description,Tags,Objects
Content-Type: application/json
Ocp-Apim-Subscription-Key: {key}

{
  "url": "https://example.com/image.jpg"
}
```
**Sample Response**
```json
{
  "description": {
    "captions": [{"text": "a dog on the grass", "confidence": 0.95}]
  },
  "tags": [{"name": "dog", "confidence": 0.98}],
  "objects": [{"object": "dog", "confidence": 0.92}]
}
```

---

### 1.2 **OCR (Optical Character Recognition)**

| **Feature**           | **OCR (Read API)** |
|-----------------------|--------------------|
| **Use Case**          | Extract text from images (e.g., scanned documents, screenshots, photos) |
| **API Variant**       | `Read` API (async model recommended) |
| **REST Flow**         | `POST /vision/v3.2/read/analyze` → `GET /vision/v3.2/read/analyzeResults/{operationId}` |
**Sample Request** 
```http
POST /vision/v3.2/read/analyze
Content-Type: application/json
Ocp-Apim-Subscription-Key: {key}

{
  "url": "https://example.com/receipt.jpg"
}
```
**Sample Final Response**
```json
{
  "analyzeResult": {
    "readResults": [
      {
        "lines": [
          { "text": "Total: $123.45", "boundingBox": [...] }
        ]
      }
    ]
  }
}
```

---

### 1.3 **Face Detection (Face API)**

| **Feature**           | **Face Detection** |
|-----------------------|--------------------|
| **Use Case**          | Detect faces and their attributes (age, emotion, head pose) |
| **Service Note**      | Separate endpoint: `https://<region>.api.cognitive.microsoft.com/face/v1.0` |
| **Auth**              | Same key + endpoint model, but Face API is a distinct service |
| **SDK**               | `azure-cognitiveservices-vision-face` |
| **Core SDK Method**   | `.detect_with_url()` |
**Sample Request** 
```http
POST /face/v1.0/detect?returnFaceAttributes=age,emotion
Content-Type: application/json
Ocp-Apim-Subscription-Key: {key}

{
  "url": "https://example.com/people.jpg"
}
```
**Sample Response**
```json
[
  {
    "faceId": "...",
    "faceRectangle": { "top": 50, "left": 100, "width": 90, "height": 90 },
    "faceAttributes": {
      "age": 34.0,
      "emotion": { "happiness": 0.98 }
    }
  }
]
```

---

### 2. **Custom Vision (Prediction)**

| **Aspect**            | **Details** |
|-----------------------|-------------|
| **Service Name**      | Azure AI Vision – Custom Vision (Prediction) |
| **Primary Use**       | Image classification and object detection using your own trained models |
| **Auth/Region**       | Project-specific endpoint; prediction key in header |
| **SDK**               | `azure-cognitiveservices-vision-customvision` |
| **Core SDK Methods**  | `.classify_image()`, `.classify_image_url()`, `.detect_image()` |
| **REST Endpoint**     | 
```http POST /customvision/v3.0/Prediction/{projectId}/classify/iterations/{iterationName}/image ```
| **Key Inputs**        | Binary image stream or image URL |
**Sample Response**
```json
[
  {
    "tagName": "Rose",
    "probability": 0.95,
    "boundingBox": null
  }
]
```

---

## 📹 **Video**

### 3. **Video Indexer**

| **Aspect**            | **Details** |
|-----------------------|-------------|
| **Service Name**      | Azure Video Indexer |
| **Primary Use**       | Deep video insights – face detection, transcript, scene segmentation, OCR, labels |
| **Auth Quirk**        | Requires session-based access token (not ARM) |
| **Steps Overview**    | 1. Get token → 2. Upload video → 3. Get insights |
| **SDK**               | None – REST only |
| **Upload Sample**     |```http POST /{location}/Accounts/{accountId}/Videos?accessToken={token}&name=demo&videoUrl=https://...```
**Insights Sample**
```json
{
  "videos": [{
    "faces": [{"name": "John", "appearances": [...] }],
    "transcript": [{"text": "Welcome to the event"}],
    "labels": ["conference", "crowd"]
  }]
}
```
