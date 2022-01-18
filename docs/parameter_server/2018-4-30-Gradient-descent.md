---
layout: default
title: Gradient descents in machine learning
parent: Parameter Server
nav_order: 2
---

**Gradient descent** is an optimization algorithm to find the parameters (i.e., weights or coefficients) of a machine learning model. It works by having the model to make prediction on the training data and using the error on the prediction to update the model parameters.
The goal of gradient descent is to minimize the error of the model on the training data set. It does this by making changes on the model to move it along a gradient or slope of errors down towards the minimal error value.

Here is a pseudocode of gradient descent algorithm:
```
model = initialization(...)
n_epochs = ...
train_data = ...
for i in n_epochs:
	train_data = shuffle(train_data)
	X, y = split(train_data)
	predictions = predict(X, model)
	error = calculate_error(y, predictions)
	model = update_model(model, error)  
```

There are three types of gradient descent:

## Stochastic Gradient Descent

**Stochastic Gradient Descent (SGD)** calculates the error and updates the model for each example of the training data set. It is also called **online machine learning algorithm**

**Upsides**

* The frequent updates immediately give an insight into the performance of the model and the rate of improvement.
* The increased model update frequency can result in faster learning on some problems.
* The noisy update process can allow the model to avoid local minima (e.g. premature convergence).

**Downsides**

* Updating the model so frequently is more computationally expensive than other configurations of gradient descent, taking significantly longer to train models on large datasets.
* The frequent updates can result in a noisy gradient signal, which may cause the model parameters and in turn the model error to jump around (have a higher variance over training epochs).
* The noisy learning process down the error gradient can also make it hard for the algorithm to settle on an error minimum for the model.

## Batch Gradient Descent

It calculates the error on each example in the training data set but updates the model after all training examples have been evaluated, i.e., an **epoch**.

**Upsides**

* Fewer updates to the model means this variant of gradient descent is more computationally efficient than stochastic gradient descent.
* The decreased update frequency results in a more stable error gradient and may result in a more stable convergence on some problems.
* The separation of the calculation of prediction errors and the model update lends the algorithm to parallel processing based implementations.

**Downsides**

* The more stable error gradient may result in premature convergence of the model to a less optimal set of parameters.
* The updates at the end of the training epoch require the additional complexity of accumulating prediction errors across all training examples.
* Commonly, batch gradient descent is implemented in such a way that it requires the entire training dataset in memory and available to the algorithm.
* Model updates, and in turn training speed, may become very slow for large datasets.

## Mini-batch Gradient Descent

It splits the training data set into small batches according to batch size, and then it calculates the error on it and updates the model after each mini batch. 
Mini-batch gradient descent seeks to find a balance between the robustness of SGD and the efficiency of batch gradient descent. It is the most common implementation of gradient descent algotithm in the field of deep learning.

**Upsides**

* The model update frequency is higher than batch gradient descent which allows for a more robust convergence, avoiding local minima.
* The batched updates provide a computationally more efficient process than stochastic gradient descent.
* The batching allows both the efficiency of not having all training data in memory and algorithm implementations.

**Downsides**

* Mini-batch requires the configuration of an additional “mini-batch size” hyperparameter for the learning algorithm.
* Error information must be accumulated across mini-batches of training examples like batch gradient descent.
