# Azure AI Services — Images & Video Overview 
### An AI-102 Mental Map
## 🖼️ **Images**
### 1. **Azure AI Vision (Computer Vision API)**

| **Aspect**            | **Details** |
|-----------------------|-------------|
| **Service Name**      | Azure AI Vision |
| **Primary Use**       | Image analysis using pretrained models (tags, objects, OCR, landmarks, etc.) |
| **Regioning/Auth**    | Key + endpoint (no token exchange); region-specific endpoint like `https://<region>.api.cognitive.microsoft.com/` |
| **Notes**             | Stateless API; handles OCR, image description, faces, objects, categories |
| **SDK**               | `azure-cognitiveservices-vision-computervision`<br>`ComputerVisionClient` |
| **Core SDK Method**   | `.analyze_image()` or `.analyze_image_in_stream()` |
| **REST Endpoint**     | `POST /vision/v3.2/analyze` |
| **Key Inputs**        | `visualFeatures`, `details`, `language`, `url` or image stream |
| **Sample Request**    | ```json { "url": "https://example.com/image.jpg" }```
| **Query Parameters**  | `visualFeatures=Tags,Description,Faces,Objects`, etc. |
| **Sample Response**   |
```json
{
  "description": {
    "captions": [{"text": "a dog on grass", "confidence": 0.95}]
  },
  "tags": [
    {"name": "dog", "confidence": 0.98},
    {"name": "grass", "confidence": 0.94}
  ]
}
```
### 2. **Custom Vision (Prediction)**

| **Aspect**            | **Details** |
|-----------------------|-------------|
| **Service Name**      | Azure AI Vision – Custom Vision (Prediction) |
| **Primary Use**       | Image classification and object detection using custom-trained models |
| **Regioning/Auth**    | Project-specific endpoint (`<region>.api.cognitive.microsoft.com`); prediction key passed in header |
| **Notes**             | Requires project + published iteration ID; used for model inference only (not training) |
| **SDK**               | `azure-cognitiveservices-vision-customvision`<br>`CustomVisionPredictionClient` |
| **Core SDK Methods**  | `.classify_image()`, `.classify_image_url()`, `.detect_image()` |
| **REST Endpoint**     | ``` POST /customvision/v3.0/Prediction/{projectId}/classify/iterations/{iterationName}/image ```
| **Key Inputs**        | Binary image stream or image URL |
| **Sample Request** (binary) | Image file as body, with header:<br>`Content-Type: application/octet-stream` |
| **Sample Response**   |
```json
[
  {
    "tagName": "Rose",
    "probability": 0.95,
    "boundingBox": null
  }
]
```
### 3. **Form Recognizer (Document Intelligence)**

| **Aspect**            | **Details** |
|-----------------------|-------------|
| **Service Name**      | Azure AI Document Intelligence |
| **Primary Use**       | Extracting structured data from documents (PDF, image) using pretrained or custom models |
| **Regioning/Auth**    | Endpoint and key; no token exchange |
| **Notes**             | Also supports custom models for structured layouts; new v3+ models support JSON schema output |
| **SDK**               | `azure-ai-formrecognizer`<br>`DocumentAnalysisClient` |
| **Core SDK Method**   | `.begin_analyze_document("prebuilt-document", ...)` |
| **REST Endpoint**     |```POST /formrecognizer/documentModels/prebuilt-document:analyze?api-version=2023-07-31```
| **Key Inputs**        | Image or PDF (binary or URL); header: `Content-Type: application/json` or `application/octet-stream` |
| **Sample Request**    |```json { "urlSource": "https://example.com/invoice.png" }```
| **Sample Response** (excerpt) |
```json
{
  "documents": [
    {
      "fields": {
        "CustomerName": { "content": "John Doe" },
        "Total": { "content": "$1,234.56" }
      }
    }
  ]
}
```
## 📹 **Video**
### 4. **Video Indexer**

| **Aspect**            | **Details** |
|-----------------------|-------------|
| **Service Name**      | Azure Video Indexer |
| **Primary Use**       | Video content analysis – face detection, transcript, scene segmentation, OCR, keywords, labels |
| **Regioning/Auth**    | Unique: access token required per session/user; not via ARM auth. Must fetch a short-lived token. |
| **Notes**             | Composite AI service – bundles multiple modalities; asynchronous indexing after upload |
| **SDK**               | No SDK – REST API only |
| **Auth Step 1**       | `GET /Auth/{location}/Accounts/{accountId}/AccessToken` (requires API key in headers) |
| **Auth Headers**      | `Ocp-Apim-Subscription-Key: {your_key}` |
| **Upload Endpoint**   | ```POST /{location}/Accounts/{accountId}/Videos```
| **Key Upload Params** | `name`, `videoUrl`, `language`, `accessToken` |
| **Sample Upload Request** |```http POST https://api.videoindexer.ai/{location}/Accounts/{accountId}/Videos?accessToken={token}&name=demo-video&videoUrl=https://...&language=English```
| **Insight Retrieval** |```GET /{location}/Accounts/{accountId}/Videos/{videoId}/Index```
| **Sample Response** (excerpt) |
```json
{
  "videos": [{
    "faces": [{"name": "John", "appearances": [...] }],
    "transcript": [{"text": "Welcome to the event"}],
    "labels": ["conference", "crowd"],
    "brands": ["Microsoft"]
  }]
}
```
