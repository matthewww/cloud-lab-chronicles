As of April 2025, there's a lot of changes happening with the Azure AI services and certification. This means many training materials are out of date or confusing at best.

# Cognitive Services -> Azure AI Services
This naming change happened around 2023/2024 but you'll still see references to cognitive services in many URLs, services and learning materials.

# Certification changes 30th April 2025 

## Primary Services
- Azure AI Vision
- Azure AI Language
- Azure AI Speech
- Azure AI Search
- Azure AI Document Intelligence
- Azure OpenAI Service
- Azure AI Content Understanding <- **New**
- Azure AI Foundry <- **New**

## Skills Measured

- Generative AI, Agents, Knowledge Mining, Planning/Managing - UP
- CV, NLP, Content Moderation - DOWN

| Subject                                                           | Pre April 2025 | Post April 2025 |
|-------------------------------------------------------------------|----------------|-----------------|
| Plan and manage an Azure AI solution                              | 15–20%         | 20–25%          |
| Implement content moderation solutions                            | 10–15%         | —               |
| Implement computer vision solutions                               | 15–20%         | 10–15%          |
| Implement natural language processing solutions                   | 30–35%         | 15–20%          |
| Implement knowledge mining and document/information extraction    | 10–15%         | 15–20%          |
| Implement generative AI solutions                                 | 10–15%         | 15–20%          |
| Implement an agentic solution                                     | —              | 5–10%           |


# Studios
- [Azure AI Foundry](https://ai.azure.com)
  - This is the main studio going forward. It's very much geared towards leveraging GenAI.

- [Copilot Studio](https://www.copilotstudio.microsoft.com)
  - LowCode Copilot / Bot builder.
  - Crossover with Bot Services but that requires more development.
  - Crossover with Agents in Azure Foundry. Copilot/Bot: Conversational Flows. Agents: GenAI driven tool workflows.

Older Studios
- [Vision Studio](https://portal.vision.cognitive.azure.com) -> Image Playground / Azure AI Content Understanding
- [Document Intelligence Studio](https://documentintelligence.ai.azure.com/) -> Azure AI Content Understanding
- [Language Studio](https://language.cognitive.azure.com) -> Language Playground
- [Speech Studio](https://speech.microsoft.com) -> Speech Playground
- [ML Studio](https://ml.azure.com/)

# Legacy Track 1 "Cognitive" -> Modern Track 2 "AI"
This can confuse SDK usage. 

Track 1 example:

`var credentials = ApplicationTokenProvider.LoginSilentAsync("tenantId", "clientId", "clientSecret").Result;`
`var client = new FormRecognizerClient(credentials) { Endpoint = "https://your-region.api.cognitive.microsoft.com" };`

Track 2 example: 

`var client = new DocumentAnalysisClient(new Uri("https://your-region.api.cognitive.microsoft.com"), new AzureKeyCredential("your-api-key"));`

| **Service**               | **Track 1 SDK**                                                                                         | **Track 2 SDK**                                                                                         |
|---------------------------|---------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| **Text Analytics**        | `Microsoft.Azure.CognitiveServices.Language.TextAnalytics`<br><br>`new TextAnalyticsClient(new ApiKeyServiceClientCredentials(key)) { Endpoint = endpoint }` | `Azure.AI.TextAnalytics`<br><br>`new TextAnalyticsClient(new Uri(endpoint), new AzureKeyCredential(key))` |
| **Computer Vision**       | `Microsoft.Azure.CognitiveServices.Vision.ComputerVision`<br><br>`new ComputerVisionClient(new ApiKeyServiceClientCredentials(key)) { Endpoint = endpoint }` | `Azure.AI.Vision.ComputerVision`<br><br>`new ComputerVisionClient(new Uri(endpoint), new AzureKeyCredential(key))` |
| **Form Recognizer**       | `Microsoft.Azure.CognitiveServices.FormRecognizer`<br><br>`new FormRecognizerClient(new ApiKeyServiceClientCredentials(key)) { Endpoint = endpoint }` | `Azure.AI.FormRecognizer.DocumentAnalysis`<br><br>`new DocumentAnalysisClient(new Uri(endpoint), new AzureKeyCredential(key))` |
| **Speech Services**       | `Microsoft.CognitiveServices.Speech`<br><br>`SpeechConfig.FromSubscription(key, region)`<br>`SpeechConfig.FromEndpoint(new Uri(endpoint), key)` <br> Or SpeechTranslationConfig
| *No Track 2 SDK available; service continues with traditional SDK structure*                             |
| **QnA Maker (Authoring)** | `Microsoft.Azure.CognitiveServices.Knowledge.QnAMaker`<br><br>`new QnAMakerClient(new ApiKeyServiceClientCredentials(key)) { Endpoint = endpoint }` | `Azure.AI.Language.QuestionAnswering`<br><br>`new QuestionAnsweringClient(new Uri(endpoint), new AzureKeyCredential(key))` |
| **Language Understanding**| `Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime`<br><br>`new LUISRuntimeClient(new ApiKeyServiceClientCredentials(key)) { Endpoint = endpoint }` | `Azure.AI.Language.Conversations`<br><br>`new ConversationAnalysisClient(new Uri(endpoint), new AzureKeyCredential(key))` |



# Form Recognizer -> Document Analysis
[FormRecognizerClient -> DocumentAnalysisClient](https://github.com/Azure/azure-sdk-for-net/blob/Azure.AI.FormRecognizer_4.1.0/sdk/formrecognizer/Azure.AI.FormRecognizer/MigrationGuide.md)
