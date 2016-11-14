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

![](./img/img-001.png) 

## Prediction of Parkinson's disease progression
In this article we are going to create a very interesting solution that can help doctors to analyse the progress of patients 
with Parkinson's disease. Our model will follow these steps:

1. Get historical data of patients;
2. Choose and apply a learning algorithm;
3. Train and test the model;
4. Predict the progression.

## What we are going to predict?
We are going to predict two scores of Unified Parkinson's disease rating scale (UPDRS): motor and total evaluation.

* **Motor UPDRS:** Score that provides a measure of key motor symptoms:

![](./img/img-002.JPG) 

* **Total UPDRS:** Sum of points based on some questions, defining the severity of the disease in a patient. 
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

## Experiments

As mentioned, this solution requires two predictions. 
For many cases you'll need to deal with only one prediction (so only one experiment is necessary).  
So for this scenario we'll need to create two experiments: one for predicting motor UPDRS and one for predicting total UPDRS. 
In the next step we are going to create a specific experiment for motor UPDRS and then create other experiment for total UPDRS.

## Getting started: Prediction for motor UPDRS
First of all, go to <a href="https://studio.azureml.net/" target="_blank">Azure ML Studio</a> website. 
If you already has an Azure subscription, you can just sign in. 
If you just want to do some tests, you can try it for free choosing the trial version.

In Azure ML Studio, create a new Blank Experiment.

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

## Choosing a learning algorithm

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

![](./img/img-008.JPG)

* **Classification:** You have a dataset and you want to identify to which set of categories the input data belongs. 
Some examples are pattern recognition and email validation ("spam" or "non-spam").

In our case we want to use *Linear Regression*, because our goal is making predictions.

## Train and test the model

We defined the learning algorithm. Now it's time to apply it to the dataset and train the model. 
Drag a **Linear Regression** and a **Train Model** item to the experiment area. 
The **Train Model** will be used to train the motor UPDRS value based on historical data.

Connect the output port of **Linear Regression** to the left input port of **Train Model** 
and connect the left output port of **Split Data** to the right input port of **Train Model**. 
The right output port of **Split Data** will be used later. The image below shows the current model.

![](./img/img-009.JPG)

As you can see the **Train Model** item is requiring a value. 
This item will be used for training the *motor_UPDRS* value based on *Linear Regression* algorithm.
So click on the Train Model item, go to *Properties* and click on *Launch column selector* button.

![](./img/img-010.JPG)

On the left, click on **With Rules** and select *motor_UPDRS* column name.

![](./img/img-011.JPG)

To avoid confusing, is a good practice defining a description for each item. To do this, double click on the Train Model 
(or any other item you want) and input a description.

![](./img/img-012.JPG)

Click on *Run* to start the training. If everything is ok, drag a **Score Model** to the experiment area. 
This item will be used to provide predictions based on the trained model. 
It will use the 25% of data that we splitted for tests previously (in the Split Data item) as input to generate predictions.

Connect the output port of the **Train Model** to the left input port of the **Score Model**. 
Also, connect the right output port of **Split Data** to the right output port of the **Train Model** item.

![](./img/img-013.JPG)

Click on *Run* to update the training. If you want to compare the actual results with predicted results, 
just right-click on the **Score Model**, select *Scored dataset* and then click on *Visualize*.

![](./img/img-014.JPG)

We just want to compare the *motor_UPDRS* with *Scored Labels*, so let's remove all other columns. 
Click on the **Score Model**, go to *Properties* and uncheck *Append score columns to output*. 
If you select *Visualize* again, you will have a visualization similar to this:

![](./img/img-015.JPG)

It is much better for comparing data, isn't it? If we compare the first row, we can see that *motor_UPDRS* (actual result) 
is equal to 8.5502 while the *Scored Labels* (predicted result) is equal to 7.766599. 
As you can see the predicted result seems good.

But how can we test the quality of the results? We can do it using an item called **Evaluate Model**. 
It is always recommended to use this item after Score Model, because it will help you see if the algorithm that you selected previously 
is good enough for your case. If results are not close to what you expected, you can easily go back and choose 
another learning algorithm and train you model again.

So drag a **Evaluate Model** to the experiment area and connect the output port of the **Score Model** to the left input port of the **Evaluate Model**. 
The image below shows the current model.

![](./img/img-016.JPG)

Click on *Run*. To see the evaluation, just right-click on the **Evaluate Model**, select *Evaluation results* and then click on *Visualize*. 

The coefficient of determination (also known as R squared value) for motor UPDRS is about 90%. 
It is a statistical metric indicating how well a model fits the data. 
For demo purposes this percentage is enough, but depending on the situation in a real case it must be improved.

![](./img/img-017.JPG)

After some tests we could see that predictions are occuring as expected. Our output on **Score Model** is returning with two columns 
(because we were comparing results previously). But now we just want prediction values as output, so let's remove the *motor_UPDRS* column.

To do this, you can remove the **Evaluate Model**, drag the **Select Columns in Dataset** item and connect it with the **Score Model**. 
The image below shows how it should be:

![](./img/img-018.JPG)

Now click on **Select Columns in Dataset**, go to *Properties* and click on *Launch column selector* button. 
On the left, click on **With Rules** and **No Columns**. Select **Include**, **column names** and enter the *Scored Labels* column name.

![](./img/img-019.JPG)

If you want to change the *Scored Labels* column name as output, you can use the **Edit Metadata** item. 

![](./img/img-020.JPG)

Click on *Run*. We finished our experiment for motor UPDRS prediction, finally!

## Deploying the Machine Learning Web Service

Now it is time to deploy our web service. 