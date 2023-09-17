# Chapter 22. Machine Learning

---
## Analyzing Text Using Amazon Comprehend, Kendra and Textract

### AWS Comprehend

AWS Comprehend uses **natural-language processing (NLP) to help you understand the meaning and sentinment in your text**, e.g. sentiment analysis.

### AWS Kendra

AWS Kendra allows you to **create an intelligent search service from different sources powered by ML**. It allows you to bridge between different silos of information, such as S3 buckets, file servers and websites, allowing your company to have all the data in one place.

### AWS Textract

AWS Textract can **process text, handwriting, tables etc using ML and optical character recognition (OCR)** with no manual interaction. You can quickly turn text into data, which you can then store in S3 or databases.

---
## Predicting Time-Series Data Using Amazon Forecast

Amazon Forecast is a time-series forecasting service that uses ML and is built to give you important business insights. You can send your data to Forecast and it will **automatically learn your data, select the right ML algorithm, and then help you forecast your data**.

---
## Protecting Accounts with Amazon Fraud Detector

Amazon Fraud Detector is a service powered by ML that is built to detect fraud in your data.

* Identify suspiciuos online payments.

* Detect new account fraud.

* Prevent Trial and Loyalty program abuse.

* Improve account takeover detection to prevent account hijacks.

---
## Working with Text and Speech Using Amazon Polly, Transcribe and Lex

Alexa uses all three components, Transcribe, Lex and Polly to create a conversational digital assistant.

### Amazon Polly

Amazon Polly **turns your text into life like speech** and allows you to create applications that talk to and interact with you using a variety of languages and accents.

### Amazon Transcribe

Amazon Transcribe is used to **convert speech to text automatically**. You can use this service to generate subtitles on the fly.

### Amazon Lex

Amazon Lex allows you to **build conversational interfaces (chatbots) in your applications using natural language models**. You can create conversational chatbots, virtual agents, or automated responses in your application.

---
## Analyzing Images via Amazon Rekognition

Amazon Rekognition is AWS computer vision product that **automates the recognition of pictures and videos **using deep learning and neural networks. You can use these processes to understand and label what is in pictures and videos.

* Content Moderation - automatically moderate content allowing your application to be family friendly.

* Face Detection and Analysis - automatically detect if someone is wearing a hat or glassses.

* Celebrity Recognition - automatically label famous people.

* Streaming Video - automatically label people and create alert notifications.

---
## Leveraging Amazon SageMaker to Train Learning Models

SageMaker is a fully managed AWS ML model creation and training tool that has automatic scaling and high availability.

Amazon SageMaker has four different areas:

* Ground Truth - set up and manage labeling jobs for training datasets using active learning and human labeling.

* Notebook - access a managed Jupyter Notebook environment.

* Training - train and tune models.

* Inference - package and deploy your ML models at scale.

### SageMaker Deployment Types

SageMaker offer two deployment types:

|                     Offline Usage                      | Online Usage                        |
|:------------------------------------------------------:|:------------------------------------|
|              Asynchronous or basic usage               | Synchronous real time               |
| Generate predictions for an entire dataset all at once | Generate low-latency predictions    |
|                 Batch transform method                 | Hosting service method              |
|          Input varies depending on algorithm           | Input varies depending on algorithm |
|          Output varies depending on algorithm          | JSON string output                  |

### SageMaker Stages

1. Load Data - Load your data from a variety of silos, such as S3, console etc.

2. Create a Model - this is the place that will provide predictions for your endpoint.

3. Create an Endpoint Configuration - specify the model to use, instance type, count, variant name and weight.

4. Create an Endpoint - this is the place where the model is published, and you can invoke it using `InvokeEndpoint()` method.

### SageMaker Neo

SageMaker Neo allows you to **customize your ML learning models for specific CPU hardware**, such as ARM, Intel and NVIDIA processors. It includes a compiler to convert the ML model to an environment that is optimized to execute the model on the target architecture.

### SageMaker Elastic Inference

Elastic Inference (EI) **speeds up throughput and decreases latency of real-time inferences** deployed on SageMaker hosted services using only CPU-based instances. It is much more cost effective than a full GPU instance.

---
## Translating Content into Different Languages with Amazon Translate

Amazon Translate is a ML service that allows you to **automate language translation**. It uses deep learning and neural networks to translate from one language to another.

* Accurate - translation service is accurate.

* Integrate - easily integrate the service with your applications using APIs.

* Cost Effective - cheaper than human translators.

* Scalable - scale with your demands.