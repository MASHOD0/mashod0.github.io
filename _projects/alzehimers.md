---
layout: page
title: Early diagnosis of Alzehiemer’s disease Application
description: Mini Project - 6th semester.
importance: 1
category: College
---

# Abstract
In this Project we explore various methods of early diagnosis of Alzheimer’s Disease.  Alzheimer’s is caused by accumulation of protein material in the brain, there are two main methods for detecting Alzheimer's. EEG scan data and MRI scan data.

We have used the MRI scan data in the format of ‘.jpg’ images for classification between the different stages of dementia. For this we have created a Flask application to serve the model which was created after experimenting with the various pre-trained models and transfer learning to fit the data to the model . 
For the final version we have created an Xception model and 2 dense layers with ReLu Activation function. In our experimentation this model performed better than other convolution based neural networks: Inception V3 and ResNet. 
We also looked at various EEG to graph  conversion techniques and various graph neural network models including Transformer based methods for graph neural networks.
We have also explored the various python libraries used for building convolutional neural networks and graph neural networks to implement the project. We also explored the various techniques for model serving and found creating a web application through the flask python web development framework to be the best fit for our use case. 	
