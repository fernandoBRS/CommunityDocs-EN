# Cloud-based Predictive Analytics with Azure Machine Learning

What if you could predict stock prices based on market movement? Or if patients would be at risk of developing certain conditions (like diabetes, asthma and heart disease)? Or even if you could detect fraud?

Some Machine Learning techniques can be used to make such predictions. In this article I will show how you can create a cloud-based predictive analytics solution with Azure Machine Learning.

## What is Machine Learning?

In few words, Machine Learning is the field of study that gives computers the ability to learn without being explicitly programmed. 
The basic idea is finding meaningful patterns in historical data and use it to solve problems.

Problems can be divided into two groups - those that can be solved using standard methods, and those that cannot be solved using standard methods.
For the first group, you have scenarios that based on inputs you know exactly the output (or what is expected as a result).
For the second group you have scenarios that the output is totally unpredictable, like the examples that I mentioned previously.

Unfortunately, most of the real life problems belongs to the second group. 
That's why Machine Learning has been so important and so studied in recent years.

## Microsoft Azure Machine Learning

Azure Machine Learning is a fully managed cloud service that enables you to easily build, deploy, and share predictive analytics solutions.
There is a web-based IDE called <a href="https://studio.azureml.net/" target="_blank">Azure Machine Learning Studio</a> 
for development and deployment of models as web services. 
You can consume these web services using some programming languages such as C#, R and Python.

![](./img/img-001.JPG) 

## Getting started
First of all, go to <a href="https://studio.azureml.net/" target="_blank">Azure ML Studio</a> website. 
If you already has an Azure subscription, you can just sign in. 
If you just want to do some tests, you can try it for free choosing the trial version.

## Prediction of Parkinson's disease progression
In this article we are going to create a very interesting solution that can help doctors to analyse the progress of patients 
with Parkinson's disease. Our model will follow these steps:

1. Get historical data of patients;
2. Choose and apply a learning algorithm;
3. Train the model;
4. Predict the progression.

## What we are going to predict?
We are going to predict two scores of Unified Parkinson's disease rating scale (UPDRS): motor and total evaluation.

* Motor UPDRS: Score that provides a measure of key motor symptoms.

![](./img/img-002.JPG) 

* Total UPDRS: Sum of points based on some questions, defining the severity of the disease in a patient. 
The score range is 0 (not affected) to 176 (most severely affected).

You can find detailed infomation about UPDRS <a href="http://viartis.net/parkinsons.disease/UPDRS2.pdf" target="_blank">here</a>.

## About the Dataset
The dataset that we are going to use as sample is composed of a range of biomedical voice measurements 
from 42 people with early-stage Parkinson's disease recruited to a six-month trial. 
It is important to mention that this dataset doesn't have any sensitive data, such as personal information, 
IDs, account numbers and so on. 

All data are stored in a CSV file (as you can see in the image below) and you can download it from 
<a href="https://archive.ics.uci.edu/ml/datasets/Parkinsons+Telemonitoring" target="_blank">UCI Machine Learning Repository</a> website.
In this page you can see all attribute information and metadata, so I will not discuss about 
all of them because this is not the objetive of this article.

![](./img/img-003.JPG) 



