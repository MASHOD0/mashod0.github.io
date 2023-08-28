---
layout: page
title: Image Captioning Algorithm 2
description: Improving On image captioning algorithm to make the captions more social media friendly.
importance: 2
category: Personal
---
# Image caption 2.0

## What's New ?
- Breakaway from the old simple descriptions to generating catchy captions for those instagram worthy posts 
### How did I do it ?
![image](https://user-images.githubusercontent.com/63853764/226140082-8a04cf67-c5d5-4a12-b058-bf8fff41c603.png)
- Used `Davinci-003` which is OpenAIs Large Language model based on the GPT-3.5 model architecture.
### How is this better ?
- Organic looking captions for instagram or linkedin images
    -   before : `this is a picture of a soccer player running with a soccer ball in his hand`
    -   after : `If you're not sweating, you're not working hard enough!`
- Customized captions for each social media platform to get the right impact
    - Instagram: `If you're not sweating, you're not working hard enough!`
    - LinkedIn: `There's nothing like a game of soccer to get the blood pumping!`
    - Twitter: `He's running with a soccer ball… and he's not looking back!`
- Faster than than the previous version roughly **7.2%** faster than the previous version 
- old outputs: 
```
filename : Image1.png
this is a picture of a soccer player running with a soccer ball in his hand
there is a man that is running with a soccer ball in his hand
this is a picture of a soccer player running with the ball in his hand
this is a picture of a soccer player running with a soccer ball in front of him
there is a man that is running with a soccer ball in front of him
filename : Image2.png
there are two horses that are standing next to each other in a field
there are two horses that are standing next to each other in the middle of a field
there are two horses that are standing next to each other in the middle of the field
there are two horses that are standing next to each other in the field
there are two horses that are standing next to each other on a field
filename : Image3.png
this is an image of a group of people who are looking at each other
this is an image of a group of four people who are looking at each other
this is an image of a group of people who are looking at the camera
this is an image of a group of four people who are looking at the same time of the day
this is an image of a group of four people who are looking at the same time of day
execution time: 80.84076929092407
```
- new outputs
```
description: ['there is a man that is running with a soccer ball in his hand']
platform: instagram
captions: 
1. Just another day on the pitch!
2. Running with the ball – gotta love soccer!
3. Never give up on your dreams!
4. If you're not sweating, you're not working hard enough!
5. Soccer is my life!

description: ['there are two horses that are standing next to each other in a field']
platform: instagram
captions: 

1. "Best friends forever!"
2. "There's no place like home."
3. "A horse is a horse, of course, of course."
4. "I'm a little horse of a different color."
5. "We're two of a kind!"

description: ['this is an image of a group of people who are looking at each other']
platform: instagram
captions: 
1. Connection is key.
2. United we stand.
3. strength in numbers
4. A team that communicate well is a team that succeeds.
5. Building relationships is the foundation of any successful venture.
total tokens: 171
execution time: 75.04193019866943
```

## Problem Statement
- Create an AI tool that creates captions based on the image provided by the user. Should also have the option to generate multiple captions based on the image.
- Provide an interface where the user can come and upload images and get AI generated captions.

## Solution
### Pre-Processing
- Used the [Blip processor](https://huggingface.co/docs/transformers/v4.26.1/en/model_doc/blip#transformers.BlipProcessor), for processing the image.
### Models
#### 1. Train a  model on the Fliker8k dataset
- Train a image captioning model using CNN and Transformer architecture on the Fliker8k dataset.
- [Fliker8k](https://github.com/jbrownlee/Datasets/releases/download/Flickr8k/Flickr8k_Dataset.zip) dataset contains 8092 images and 5 captions for each image.
- Disadvantages:
    - The dataset is small and the model will not be able to generalize well.
    - The model will not be able to generate captions for images that are not present in the dataset.
    
#### 2. The `nlpconnect/vit-gpt2-image-captioning` model
- Use the `nlpconnect/vit-gpt2-image-captioning` model to generate captions for the images.
- [nlpconnect/vit-gpt2-image-captioning](https://huggingface.co/nlpconnect/vit-gpt2-image-captioning) model is one of the most downloaded model on the huggingface model hub. It has over 1.1 million downloads.
- [the script used in generating the results](nlpconnect-vit-gpt-2-captioning.py)

##### Results of the `nlpconnect/vit-gpt2-image-captioning` model
- `temprature` adjustments : (https://docs.google.com/spreadsheets/d/1yz25PL-s2VbGVhij0wr9SMotuXexYrgGU-svi4aFIic/edit?usp=sharing)
- experimentation with `top_k` parameter: (https://docs.google.com/spreadsheets/d/1Wfhxj-4AX5WQpGO2v_0B9e0NspoHOsERJw229llngbY/edit?usp=sharing)
- results of the full test of selected `temprature` and `top_k` values:(https://docs.google.com/spreadsheets/d/1Gb1XxMa3S2hSjamPjVTGfNRNz83Ff67HWuOXzQstEIU/edit?usp=sharing)
![image](https://user-images.githubusercontent.com/63853764/221746440-deb4823f-bdb0-4275-8938-343d1da2b53c.png)
- The generated sentences were not good enough.

#### 3. The ` Salesforce Blip image captioning large` model
- **BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation** is a pre-trained model for image captioning.
- The model is trained on the [MS COCO](https://cocodataset.org/#home) dataset.
- The model is trained on the `large` version of the [ViT](https://arxiv.org/abs/2010.11929) architecture.
- This model is selected for the Implementation as the captions generated from this are good
- [Script used for experimentation ](salesforce_blip_image_captioning_large.py)

### Model evaluation
- Simple intuition was used here to evaluate the outputs of various models as the difference between the accuracy of the models was clearly visible.
![image](https://user-images.githubusercontent.com/63853764/221748485-4c1a2481-679a-48e2-b69a-8c4ca3454864.png)
- For example, for the above image the following are sentences generated by `nlpconnect/vit-gpt2-image-captioning` and ` Salesforce Blip image captioning large` models
    - `nlpconnect/vit-gpt2-image-captioning`: a woman in brown outfit holding a white horse behind it in dark cloudy sky
    - ` Salesforce Blip image captioning large` : there are two horses that are standing next to each other in a field
   
### Interface
#### 1.CLI
- A Simple command line application that can be run by running [`runner.py`](/runner.py)
- Prompts the user for location of the folder and number of captions to generate.
- Generates the captions and calculates the time taken to generate the results.
- Suitable for batch generation of captions as it can take multiple images at once
- Selected for implementation
#### 2.GUI
- A web application implemented using frameworks like flask.
- Prompts the user to upload the files, generates and displays the captions on the web interface.
- Requires UI Design and file handling.

### Implementation
#### Data Files
```
📦listed_image_caption
 ┃ 
 ┗ 📂inputs
   ┣ 📜Image1.png
   ┣ 📜Image2.png
   ┗ 📜Image3.png
 
 ```
 #### Code files
 ```
📦listed_image_caption
 ┃
 ┣ 📜Model.py
 ┣ 📜nlpconnect-vit-gpt-2-captioning.py
 ┣ 📜runner.py
 ┗ 📜salesforce_blip_image_captioning_large.py
 ```
 ##### `Model.py`
 Implements the `predict()` which takes the image and number of sequences as input and generates the captions.
 ##### `runner.py`
 Implements the interface of the application.
 ##### `nlpconnect-vit-gpt-2-captioning.py`
 Experiments with the nlpconnect-vit-gpt-2-captioning model.
 ##### `salesforce_blip_image_captioning_large.py`
 Experiments with the salesforce_blip_image_captioning_large model
 

## Improvements and Conclusion
- Create a GUI user interface using flask or tkinter, this will make the application more user friendly.
- The model can be improved by using a larger dataset and training it for a longer period of time.
- The runtimes can be improved by using a GPU.
- Using metrics like BLEU score for model evaluation.
