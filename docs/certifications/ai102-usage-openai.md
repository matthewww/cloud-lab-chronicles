## Azure OpenAI: SDK + REST API Comparison

1. **API endpoint structure**
2. **Input types (text prompt, file, image, function calls)**
3. **SDK vs REST comparison**
4. **Authentication and regioning**
5. **Sample request payloads**
6. **Typical responses**

---

## 1. Regional Endpoint Format

**General Azure OpenAI Endpoint Pattern:**
```
https://<resource-name>.openai.azure.com/openai/deployments/<deployment-id>/<operation>?api-version=<version>
```

| Component           | Meaning                                         |
|---------------------|-------------------------------------------------|
| `<resource-name>`   | Your Azure OpenAI resource name                 |
| `<deployment-id>`   | Custom name given to a deployed model           |
| `<operation>`       | `chat/completions`, `completions`, etc.        |
| `api-version`       | Example: `2024-02-15-preview`                  |

**Authentication:**
- Use `api-key` from Azure resource
- Optional: `AAD token` with `Bearer` header

---

## 2. `chat/completions` (ChatGPT-style models)

### SDK (Python)
```python
from openai import AzureOpenAI

client = AzureOpenAI(
    api_key="your-key",
    api_version="2024-02-15-preview",
    azure_endpoint="https://<resource-name>.openai.azure.com"
)

response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Tell me about Azure AI."}
    ],
    temperature=0.7,
    max_tokens=256,
    stream=False
)
```

### REST
```http
POST https://<resource>.openai.azure.com/openai/deployments/<deployment-id>/chat/completions?api-version=2024-02-15-preview

Headers:
  api-key: <your-key>
  Content-Type: application/json

Body:
{
  "messages": [
    { "role": "system", "content": "You are a helpful assistant." },
    { "role": "user", "content": "Tell me about Azure AI." }
  ],
  "temperature": 0.7,
  "max_tokens": 256
}
```

---

### Response Structure
```json
{
  "id": "chatcmpl-123",
  "object": "chat.completion",
  "created": 1711234567,
  "model": "gpt-4",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "Azure AI provides..."
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 24,
    "completion_tokens": 42,
    "total_tokens": 66
  }
}
```

---

## 3. `completions` (Legacy Text-Davinci style models)

> Mostly replaced by chat models, but still testable.

### REST Example
```http
POST https://<resource>.openai.azure.com/openai/deployments/<deployment-id>/completions?api-version=2024-02-15-preview

Body:
{
  "prompt": "Explain transformers in AI.",
  "max_tokens": 200,
  "temperature": 0.5
}
```

---

## 4. `embeddings`

Used for vector search, semantic similarity, retrieval-augmented generation (RAG), etc.

### REST Example
```http
POST https://<resource>.openai.azure.com/openai/deployments/<deployment-id>/embeddings?api-version=2024-02-15-preview

Body:
{
  "input": ["Climbing in the Valley of 1000 Hills"],
  "user": "matt-user"
}
```

**Response:**
```json
{
  "data": [
    {
      "embedding": [0.01, 0.22, ...],
      "index": 0
    }
  ],
  "usage": {
    "prompt_tokens": 8,
    "total_tokens": 8
  }
}
```

---

## 5. `images/generations` (DALL·E)

Only available via **preview** and some SKUs.

```http
POST https://<resource>.openai.azure.com/openai/images/generations:submit?api-version=2024-02-15-preview

Body:
{
  "prompt": "a white van on a mountain trail in South Africa",
  "n": 1,
  "size": "1024x1024"
}
```

Response includes a `url` for the generated image.

---

## 6. `audio/transcriptions` (Whisper)

Supports speech-to-text. Upload via file stream.

```http
POST https://<resource>.openai.azure.com/openai/deployments/<deployment-id>/audio/transcriptions?api-version=2024-02-15-preview

Headers:
  Content-Type: multipart/form-data

Form-Data:
  file: (binary audio file)
  model: "whisper-1"
```

---

## 7. Fine-tuning (Outline only)

- Upload training files via Azure OpenAI Studio or API
- Launch training job via `fine-tunes` endpoint
- Only some models (e.g., `davinci`, `curie`) support fine-tuning

Not core for AI-102 *unless you're asked to explain the difference between fine-tuning and prompt engineering*.

---

## 8. Token Management (for REST)

| Auth Type     | Header                        | Note                             |
|---------------|-------------------------------|----------------------------------|
| API Key       | `api-key: <your-key>`         | Preferred for quick tests        |
| Azure AD      | `Authorization: Bearer <JWT>` | Needed for RBAC + managed access |

---

## Summary Table

| Task                  | Endpoint                              | Input Style                  | SDK Call                               | Key Params                        |
|-----------------------|----------------------------------------|------------------------------|----------------------------------------|-----------------------------------|
| Chat Completion       | `/chat/completions`                   | `messages[]`                 | `client.chat.completions.create(...)`  | `temperature`, `max_tokens`, etc. |
| Completion (legacy)   | `/completions`                        | `prompt`                     | `client.completions.create(...)`       |                                   |
| Embedding             | `/embeddings`                         | `input` (string or list)     | `client.embeddings.create(...)`        |                                   |
| Image Generation      | `/images/generations:submit`          | `prompt`                     | Not in standard SDK yet (REST only)    | `size`, `n`                       |
| Audio Transcription   | `/audio/transcriptions`               | binary file                  | Not in SDK (use requests/multipart)    | `language`, `prompt`              |
| Fine-tuning           | `/fine-tunes`                         | uploaded files               | CLI or REST                            |                                   |
