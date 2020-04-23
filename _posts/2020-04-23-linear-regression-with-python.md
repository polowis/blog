---
date: 2020-04-23 04:41:46
layout: post
title: "Linear Regression with python"
subtitle:
description:
image: https://mobidev.biz/wp-content/uploads/2020/01/machine-learning-methods-demand-forecasting-1.png
optimized_image:
category: code
tags:
    - machinelearning
    - python
author: polowis
paginate: false
---

There are two types of supervised machine learning algorithms which are regression and classification.
Linear regression is a machine learning algorithm. Most of the time, it is used to find out the relationship between input variables (x) and output variables (y). If you plot x on the x-axis and the y on the y-axis, linear regression is the line that best fit the data points as show below. This article isn't about explanation of linear regression. Others might have better explanations than me. 

<img src="/assets/img/uploads/linear1.png">

## Simple linear regression

Let's start with the dataset. For this simple task, we're going to predict the student's score based on hours of study, instead of using the popular Boston house price task. The information in this dataset is not much to say, it only contains the scores and the hours of study. Obviously, you can make it up your own.

Before we dive into coding, you might want to import some neccessary libraries.

```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn import metrics

```
We will start with a simple way of implementing linear regression

1. ```numpy```. A powerful math library to do vectorized numerical calculations.
2. ```matplotlib``` This will helps you plot the graph to gain a better understanding
3. ```pandas``` A library that helps you to read csv files.
4. ```sklearn``` A package that implements multiple machine learning algorithms.


Let's start by importing the dataset.
```py
dataset = pd.read_csv('student_scores.csv')
```

Then you might want to explore the dataset by using this ```describe()``` function.

```py
dataset.describe()
```
You will be able to see something like this

<img src="/assets/images/uploads/linear2.png">

Finally, you might want to to plot your data using ```matplotlib``` so you can find the relationship between data points

```py
dataset.plot(x='Hours', y='Scores', style='o')
plt.title('Hours vs Percentage')
plt.xlabel('Hours Studied')
plt.ylabel('Percentage Score')
plt.show()
```

The result will be displayed like this:

<img src="assets/images/uploads/linear3.png">


Our next step is to divide the data into “attributes” and “labels”. 
**Attributes** are the independent variables and **labels** are dependent variables whose values are to be predicted. In our dataset, we only have two columns, we want to predict the scores depending on the hours of study. Therefore our independent variables would be the hours of study (x) and the label will be the **score** which is stored in y variable.


```py
X = dataset['Hours'].values.reshape(-1,1)
y = dataset['Scores'].values
```

Now you might want to split the 80% of the data into the training set, and 20% of the data will be used to test. 

```py
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
```

After splitting the data into training and testing sets, it is now the time to train the algorithm. You will need to initialize ```LinearRegression``` class, and call the fit functions with our trained data.

```py
regressor = LinearRegression()
regressor.fit(X_train, y_train)
```
After you train the dataset, you can start predict using ```predict()``` function and see how accurately the algorithm predicts the percentage score.

```py
y_pred = regressor.predict(X_test)
```

Next, you can compare the predicted values with the actual values. 

```py
df = pd.DataFrame({'Actual': y_test, 'Predicted': y_pred})
print(df)
```

<img src="assets/images/uploads/linear4.png">

And, finally, plot our straight line

```py
dataset.plot(x='Hours', y='Scores', style='o')
plt.plot(X_test, y_pred, color='red', linewidth=2)
plt.title('Hours vs Percentage')
plt.xlabel('Hours Studied')
plt.ylabel('Percentage Score')
plt.show()

```

<img src="/assets/img/uploads/linear1.png">

### How do we know how well the algorithm performs?

Before answer the questions, let's try to understand what was going on here. How the linear regression class works under the hood.

We need to evaluate the performance of the algorithms so we can adjust and see if we can improve it. This process is called ```Cost function```. There are 3 metrics commonly used to evaluate. 

##### Mean Absolute Error (MAE)

The mean of the absolute value of the error
<img src="https://miro.medium.com/max/558/1*7ecP4al8bd3_PA2fdRYfyg.png">

##### Mean Squared Error (MSE)
The mean of the squared error

<img src="https://cdn-media-1.freecodecamp.org/images/hmZydSW9YegiMVPWq2JBpOpai3CejzQpGkNG">

##### Root Mean Squared Error (RMSE) 
The square root of the mean of the squared errors:

<img src="https://miro.medium.com/max/654/1*SGBsn7WytmYYbuTgDatIpw.gif">

Let's implement the Mean Squared Error as our cost function

```py
def compute_cost(X, y, param):
    n = len(y)
    h = X @ param
    return np.sum((h-y) ** 2)/(2*n)
```

The ```h``` variable represents our hypothesis function