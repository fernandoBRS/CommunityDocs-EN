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

## Prediction of Parkinson's disease progression
In this article we are going to create a very interesting solution that can help doctors to analyse the progress of patients 
with Parkinson's disease. Our model will follow these steps:

1. Get historical data of patients;
2. Choose and apply a learning algorithm;
3. Train and test the model;
4. Predict the progression.

## What we are going to predict?
We are going to predict two scores of Unified Parkinson's disease rating scale (UPDRS): motor and total evaluation.

* Motor UPDRS: Score that provides a measure of key motor symptoms:

![](./img/img-002.JPG) 

* Total UPDRS: Sum of points based on some questions, defining the severity of the disease in a patient. 
The score range is 0 (not affected) to 176 (most severely affected).

You can find detailed infomation about UPDRS <a href="http://viartis.net/parkinsons.disease/UPDRS2.pdf" target="_blank">here</a>.

## About the dataset
The dataset that we are going to use as sample is composed of a range of biomedical voice measurements 
from 42 people with early-stage Parkinson's disease recruited to a six-month trial. 
It is important to mention that this dataset doesn't have any sensitive data, such as personal information, 
IDs, account numbers and so on. 

All data are stored in a CSV file (as you can see in the image below) and you can download it from 
<a href="https://archive.ics.uci.edu/ml/datasets/Parkinsons+Telemonitoring" target="_blank">UCI Machine Learning Repository</a> website.
In this page you can see all attribute information and metadata, so I will not discuss about 
all of them because this is not the objetive of this article.

![](./img/img-003.JPG) 

## Getting started
First of all, go to <a href="https://studio.azureml.net/" target="_blank">Azure ML Studio</a> website. 
If you already has an Azure subscription, you can just sign in. 
If you just want to do some tests, you can try it for free choosing the trial version.

In Azure ML Studio, create a new Blank Expermiment.

![](./img/img-004.JPG)

## Get historical data of patients

Basically there are two ways of importing data:

* Upload a new dataset from local file. To do this, go to **Datasets** section and upload your CSV file.
* Point to a data source, such as: Web URL, Azure Storage, on-premises SQL database, etc.

I'll choose to point to a data source, because we already have the dataset sample on UCI webpage.
In the search box, search by **Import Data** and drag it to the experiment area. 
This item will be used to import the CSV file containing our historical data.

In *Properties*, choose *Web URL via HTTP*, provide the *Data source URL* (you can get it <a href="https://archive.ics.uci.edu/ml/machine-learning-databases/parkinsons/telemonitoring/parkinsons_updrs.data
" target="_blank">here</a>) and choose CSV as *Data format*. As you can see in our CSV file, it has header row. So check the *CSV or TSV has header row* option.

![](./img/img-005.JPG)

Click on *Run* to process the dataset. After finished running, right-click on the *Import Data* item and choose *Results dataset* and then *Visualize*.
You can see the dataset was imported successfully and now you can proceed.

![](./img/img-006.JPG)

Our historical data will be splitted in two sets: training and testing. 
The training set is used with the learning algorithm for prediction, while testing set is used as input for the model (helping you see how close the prediction is from the actual value).

A fraction of data will be redirected to training set and the other fraction will be redirected to testing set. 
To improve the quality of prediction and at the same time have good amount of data for tests, let's split 75% of our data for training set and 25% for testing set.

To do this, search by **Split Data** and drag it to the experiment area. In *Properties*, change *Fraction of rows in the first output dataset* to 0.75. 
Then connect the output of **Import Data** to the input of **Split Data**, as you can see in the image below:

![](./img/img-007.JPG)

## Applying a learning algorithm

Our dataset is ready to be used and now we need to choose the learning algorithm that fits to our needs. 
There are a lot of algorithms for different purposes. Basically these algorithms are splitted in three types:

* **Supervised Learning:** Given some inputs and desired outputs by a data source, the goal is to learn a general pattern that maps these inputs and outputs. 
So you provide the right answer in advance.

* **Unsupervised Learning:** The algorithm figure out the inputs and outputs itself. One example is *image classification* problem. 
You probably don't know what the pictures are about, so it will need to find out similarities in the input data and figure out itself 
the best way to classify the pictures into proper groups.

* **Reinforcement Learning:** Is the problem of getting an agent to act in a dynamic environment so as to maximize its rewards. 
For example, consider teaching a dog a new trick: you cannot tell it what to do, but you can reward/punish it if it does the right/wrong thing. 
It has to figure out what it did that made it get the reward/punishment. 
We can use a similar method to train computers to do many tasks, such as playing chess, driving vehicles, scheduling jobs, etc.

Supervised Learning is the type that fits in our problem. There are two techniques that can be used: *regression* and *classification*.

* **Regression:** You have a dataset and you want to use it to make predictions. A common example is predicting the price of a house given its size in feet. 
Based on historical data, the algorithm creates the line that fits better for general cases. This line corresponds to a mathematical equation. 
So, when you have an equation you can find any output (y) given any input (x). This process is known as **Linear Regression**.

* **Classification:** You have a dataset and you want to identify to which set of categories the input data belongs. 
Some examples are pattern recognition and email validation ("spam" or "non-spam").

![](./img/img-008.JPG)