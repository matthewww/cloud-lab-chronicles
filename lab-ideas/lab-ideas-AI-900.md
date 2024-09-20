# AI-900 Labs

To efficiently prepare for the exam through hands-on learning, we can create small projects or practical exercises for each of the main areas covered by the AI-900 exam.

1. Describe Artificial Intelligence Workloads and Considerations
    - Classify AI vs. Non-AI Solutions
2. Describe Fundamental Principles of Machine Learning on Azure
    - Build a Basic Predictive Model using Azure Machine Learning
3. Describe Features of Computer Vision Workloads on Azure
    - Image Classification using Azure Custom Vision
4. Describe Features of Natural Language Processing (NLP) Workloads on Azure
    - Sentiment Analysis with Azure Text Analytics
5. Describe Features of Conversational AI Workloads on Azure
    - Build a Simple FAQ Bot using Azure Bot Service and QnA Maker

---

### 1. Describe Artificial Intelligence Workloads and Considerations

**Project: Classify AI vs. Non-AI Solutions**

**Objective:** To understand the types of AI workloads and identify scenarios where AI can be used effectively.

**Steps:**
1. **Research AI Use Cases**: Find examples of AI applications in various industries (e.g., healthcare, finance, retail, etc.). Write a brief description of each use case.
2. **Identify AI Components**: For each use case, identify the AI components involved (e.g., machine learning, computer vision, natural language processing).
3. **Categorize Workloads**: Classify the workloads based on the AI solutions:
   - Knowledge mining
   - Anomaly detection
   - Prediction and forecasting
   - Natural language processing
   - Computer vision
4. **Non-AI vs. AI Solution**: Provide a real-world example of a problem that could be solved with a traditional non-AI approach versus an AI approach (e.g., rule-based vs. machine learning for spam detection).
5. **Ethical Considerations**: List potential ethical implications or considerations for each AI use case, such as bias, privacy, or security concerns.

**Tools:** Pen and paper, or a document editor like Microsoft Word or Google Docs.

**Outcome:** Develop a deeper understanding of when and how to apply AI technologies and the ethical considerations associated with AI solutions.

---

### 2. Describe Fundamental Principles of Machine Learning on Azure

**Project: Build a Basic Predictive Model using Azure Machine Learning**

**Objective:** To create a basic machine learning model to understand the process of data ingestion, model training, and evaluation on Azure.

**Steps:**
1. **Set Up Azure Machine Learning Workspace**:
   - Create an Azure Machine Learning workspace.
   - Set up the necessary environment, including compute resources.
   
2. **Data Ingestion**:
   - Use a sample dataset from Azure Open Datasets (e.g., NYC taxi fares dataset).
   - Explore and clean the data using Azure Machine Learning Designer or Python SDK in Jupyter Notebook.

3. **Model Training**:
   - Use the Azure Machine Learning Designer (drag-and-drop interface) to build a simple regression model to predict taxi fares.
   - Split the data into training and testing datasets.
   - Train the model using a linear regression algorithm.

4. **Evaluate the Model**:
   - Evaluate model performance using metrics such as Mean Absolute Error (MAE) and R-squared.
   - Tune the model by adjusting parameters to improve accuracy.

5. **Deploy the Model**:
   - Deploy the trained model as a web service on Azure.
   - Test the web service by sending a sample input and receiving the predicted output.

**Tools:** Azure Machine Learning, Azure Machine Learning Designer, Jupyter Notebook.

**Outcome:** Gain practical experience in setting up an Azure ML environment, training a basic model, and deploying it as a web service.

---

### 3. Describe Features of Computer Vision Workloads on Azure

**Project: Image Classification using Azure Custom Vision**

**Objective:** To build, train, and test a custom image classification model using Azure's Custom Vision service.

**Steps:**
1. **Set Up Custom Vision Service**:
   - Create a Custom Vision project in the Azure portal.
   - Choose a classification project type (e.g., multi-class classification).

2. **Upload and Label Images**:
   - Upload a dataset of labeled images. You can use public datasets or your own set of images (e.g., pictures of different animals or objects).
   - Ensure a balanced number of images per class.

3. **Train the Model**:
   - Train the image classification model using the labeled images.
   - Adjust training iterations for optimal performance.

4. **Evaluate Model Performance**:
   - Use the platform’s tools to evaluate model accuracy.
   - Test the model with new images to see how well it generalizes.

5. **Deploy and Test the Model**:
   - Deploy the model as an endpoint.
   - Test the endpoint using REST API or the Custom Vision portal by sending a new image and receiving the classification result.

**Tools:** Azure Custom Vision, Azure Portal, REST API tools.

**Outcome:** Understand the process of creating, training, evaluating, and deploying a computer vision model using Azure's Custom Vision service.

---

### 4. Describe Features of Natural Language Processing (NLP) Workloads on Azure

**Project: Sentiment Analysis with Azure Text Analytics**

**Objective:** To use Azure Text Analytics to analyze the sentiment of customer reviews.

**Steps:**
1. **Set Up Text Analytics Resource**:
   - Create a Text Analytics resource in the Azure portal.

2. **Prepare Dataset**:
   - Use a sample dataset of customer reviews (e.g., product reviews from Amazon or Yelp).
   - Ensure that the dataset is in a text format (CSV or JSON).

3. **Analyze Sentiment**:
   - Use the Azure Text Analytics API to analyze the sentiment of each review in the dataset.
   - Write a script in Python or use a Jupyter Notebook to send requests to the API and parse the responses.

4. **Visualize Results**:
   - Aggregate the results to see the overall sentiment distribution (positive, neutral, negative).
   - Use a visualization tool (e.g., Matplotlib or Power BI) to create a bar chart or pie chart of the sentiment analysis results.

5. **Analyze Key Phrases and Entities**:
   - Use the Text Analytics API to extract key phrases and named entities from the reviews.
   - Identify common themes or trends in customer feedback.

**Tools:** Azure Text Analytics, Python, Jupyter Notebook, data visualization tools (Matplotlib, Power BI).

**Outcome:** Learn how to perform sentiment analysis and extract key insights from text data using Azure's NLP capabilities.

---

### 5. Describe Features of Conversational AI Workloads on Azure

**Project: Build a Simple FAQ Bot using Azure Bot Service and QnA Maker**

**Objective:** To create a basic chatbot that answers frequently asked questions (FAQ) using Azure Bot Service and QnA Maker.

**Steps:**
1. **Set Up QnA Maker Resource**:
   - Create a QnA Maker resource in the Azure portal.

2. **Create a Knowledge Base**:
   - Use a pre-existing FAQ document or create one with questions and answers relevant to a specific topic (e.g., company policies, product information).
   - Import this document into QnA Maker to create a knowledge base.

3. **Train the QnA Maker**:
   - Train the QnA Maker with the uploaded questions and answers.
   - Test the knowledge base by asking sample questions to ensure it retrieves correct responses.

4. **Deploy the Knowledge Base**:
   - Publish the knowledge base to make it available for integration with the bot.

5. **Create a Bot using Azure Bot Service**:
   - Use the Azure Bot Service to create a new bot and link it to your QnA Maker knowledge base.
   - Test the bot using the Web Chat feature in the Azure portal.

6. **Enhance the Bot’s Capabilities**:
   - Add additional intents or use Language Understanding (LUIS) to enhance the bot’s ability to understand different queries.
   - Integrate the bot with other channels like Microsoft Teams, Slack, or a web application.

**Tools:** Azure Bot Service, QnA Maker, Azure Portal.

**Outcome:** Learn how to create and deploy a simple FAQ bot using Azure's Conversational AI services, and understand the basics of bot development and integration.
