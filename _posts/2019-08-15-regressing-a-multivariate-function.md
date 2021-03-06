---
layout: post
title:  "Using Machine Learning Models to Regress a Multivariate Math Function"
date:   2019-08-15 00:00:00 -0700
categories: 
---
My friend Peter is an applied mathematician who did some work in fluid dynamics. He wrote MatLab code to implement a mathematical function that calculates a maximal value of a variable K given four variables as inputs. MatLab is a numerical computing environment that includes its own programming language.

As an exploratory and fun exercise, I decided to try implementing that function using some kind of machine learning model.

For my purposes it was not necessary to understand the physics of what the inputs and the outputs mean, although it should be noted that the math is somewhat complex in that it applies polynomials in various terms of higher powers.

I just treated the MatLab code as a black box that accepts certain inputs and generates corresponding outputs. My goal was to create an ML model that acts as an equivalent black box.

Peter provided [a set of 549 inputs with their outputs]({{site.url}}/files/K_data.csv) as a representative sampling of what is generated by the MatLab code. A few of those samples are shown in the table below. **K** is the output and the other four columns are the inputs.

 <table style="width:80%; background-color:white">
	<tr><th>output: K</th><th>theta</th><th>n</th><th>m</th><th>h_star</th></tr>

	<tr><td>1.509</td><td>0.52360</td><td>3</td><td>2</td><td>0.01</td></tr>
	<tr><td>1.558</td><td>0.54105</td><td>3</td><td>2</td><td>0.01</td></tr>
	<tr><td>1.656</td><td>0.57596</td><td>3</td><td>2</td><td>0.01</td></tr>

	<tr><td>1.383</td><td>0.71558</td><td>4</td><td>3</td><td>0.01</td></tr>
	<tr><td>1.415</td><td>0.73304</td><td>4</td><td>3</td><td>0.01</td></tr>
	<tr><td>1.511</td><td>0.78540</td><td>4</td><td>3</td><td>0.01</td></tr>

	<tr><td>0.881</td><td>1.3788</td><td>9</td><td>4</td><td>0.01</td></tr>
	<tr><td>0.891</td><td>1.3963</td><td>9</td><td>4</td><td>0.01</td></tr>
	<tr><td>0.909</td><td>1.4312</td><td>9</td><td>4</td><td>0.01</td></tr>

	<tr><td>0.870</td><td>0.90757</td><td>3</td><td>2</td><td>0.001</td></tr>
	<tr><td>0.886</td><td>0.92502</td><td>3</td><td>2</td><td>0.001</td></tr>
	<tr><td>0.917</td><td>0.95993</td><td>3</td><td>2</td><td>0.001</td></tr>

	<tr><td>0.001</td><td>1.309</td><td>9</td><td>4</td><td>0.0001</td></tr>
	<tr><td>0.001</td><td>1.3265</td><td>9</td><td>4</td><td>0.0001</td></tr>
	<tr><td>0.001</td><td>1.3614</td><td>9</td><td>4</td><td>0.0001</td></tr>

	<tr><th>output: K</th><th>theta</th><th>n</th><th>m</th><th>h_star</th></tr>
 </table>

**theta** ranges continuously from 0.5326 to 1.5708. (it represents angles from 30 degrees to 90 degrees in radians.)

**n** and **m** have three possible pairs of discrete values:  (3, 2), (4, 3) and (9, 4).

**h_star** has three possible discrete values:  0.01, 0.001 and 0.0001.

The output **K** is a real number ranging continuously from 0 to about 4.1. So this can be somewhat thought of as a multi-variate regression problem with a continuous real number output.

Of the 549 examples in the data set, I used 538 for training and 11 for testing. Initially, the data set was kept in its original order but the ordering was later randomized.

The 11 test examples were chosen in a more or less stratified way in order to represent the discrete and continuous possibilities for the inputs and for the output K.

<hr width="80%" />
<br />

#### **Using neural net models** ####

Almost all of my studies in ML have been in neural nets, so that was where I started. I used the Keras framework with sequential models that make it very easy to define the layers and that automatically infer the data flows between the layers.

One of the first models simplistically had a single hidden layer of 50 densely-connected units with a densely-connected output layer of 1 unit. The loss function was mean squared error, the optimizer was Adam, and the number of epochs 150.

During training, the loss function bottomed out at a low of about 0.20. After running the trained model on the test data, the mean value of the errors of the predictions was 0.20. This was a 33% error from the correct output values. Not close at all.

As it turns out, after trying a number of neural nets I was not able to do any better. Model variations included:

* Varying the number of hidden layers (all fully connected) between one and five
* Changing the number of units in the layers
* Randomizing the order of the training data
* Increasing the number of training epochs
* Linearly scaling the four input variables to the interval [0, 1]
* Using the RMSprop optimizer instead of Adam

Here is a plot of the last run, which was very typical of the results for the neural net models. The x-axis is theta. The y-axis is the output K. The blue points are the training data, the orange points are the test data, and the red points are the test input data with their predicted values.

The other three input variables are not included, but these two-dimensional plots do show the shape of the theta variable and a visual indication of the accuracy of the results by way of the distance between each orange point and its corresponding red point.

<image src="{{site.url}}/images/theta-K-neural-net.png" alt="Plot of theta against K values for neurl net" />
<br />

<hr width="80%" />
<br />

#### **Using decision tree models** ####

While trying different neural nets, I happened to mention this exercise to Brian, a data science professor. He suggested using random forests instead.

I installed the SciKit-Learn framework, deciding to begin even more simply than random forests by creating decision tree models with the DecisionTreeRegressor class.

I varied max_depth for the models but used default values for all other parameters.

Again starting really simplistically, I created a decision tree with max_depth of 1. That resulted in predictions with a mean error of 0.54, which was a mean error percentage of 47%. 

<image src="{{site.url}}/images/theta-K-decision-tree-depth-1.png" alt="Plot of theta against K values for decision tree with depth 1" />
<br />

Next, a decision tree with max_depth of 5. I expected a good improvement and got one. The predictions had a mean error of 0.09, which was off by 7.6%.

<image src="{{site.url}}/images/theta-K-decision-tree-depth-5.png" alt="Plot of theta against K values for decision tree with depth 5" />
<br />

With a max_depth of 10, the mean error dropped to 0.017 and a mean error percentage of 1.5%. Pretty good!

<image src="{{site.url}}/images/theta-K-decision-tree-depth-10.png" alt="Plot of theta against K values for decision tree with depth 5" />
<br />

I then tried running decision trees with max_depth of 11, 20, and 50 and got exactly the same predictionas as the one with max_depth of 10. There was no improvement at all.

Future work: understand exactly how these decision trees work for this somewhat complex regression problem and in particular how max_depth is involved.

