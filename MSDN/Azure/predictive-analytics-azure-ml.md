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
There is a web-based IDE called [Azure Machine Learning Studio](https://studio.azureml.net/) 
for development and deployment of models as web services. 
You can consume these web services using some programming languages such as C#, R and Python.

![](./img/pic-001.jpg) 

## Getting started
First of all, go to [Azure ML Studio](https://studio.azureml.net/) website. 
If you already has an Azure subscription, you can just sign in. 
If you just want to do some tests, you can try it for free choosing the trial version.

## Prediction of Parkinson's disease progression
In this article we are going to create a very interesting solution that can help doctors to analyse the progress of patients 
with Parkinson's disease. Our model will follow these steps:

1. Get historical data of patients;
2. Choose and apply a learning algorithm;
3. Train the model;
4. Predict the progression.

## About the Dataset
The dataset that we are going to use as sample is composed of a range of biomedical voice measurements 
from 42 people with early-stage Parkinson's disease recruited to a six-month trial.

It is important to mention that this dataset doesn't have any sensitive data, such as personal information, 
IDs, account numbers and so on.

All data are stored in a CSV file and you can download it from 
[UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Parkinsons+Telemonitoring) website.