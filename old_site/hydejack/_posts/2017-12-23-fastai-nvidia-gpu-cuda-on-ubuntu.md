---
layout: post
title:  "Learnings from State Farm Distracted Driver dataset on Kaggle"
date:   2017-12-23 18:16:01 +0530
categories: [Deep Learning]
tags: [AI, nvidia, cuda, ubuntu, fastai]
comments: true
---

I recently came across [Fast.AI](http://fast.ai), an online deep learning course. Being describe as no BS, practical course, I decided to give it a try. I'm on my 3rd lecture and have learned some nice concepts like finetuning, activation functions and some cool linux tricks too.

As recommended in the course, I decided to try my hand at the [State Farm Distracted Driver competition on Kaggle](https://www.kaggle.com/c/state-farm-distracted-driver-detection). This is basically an image classification competition with 10 categories. Each category has images of drivers, all shot from the same angle. The drivers in different categories are disctrated in different manners. This is what we need to identify. Each image is 480 x 640 in size.

I tried to use the VGG16 model that had been used the the course for Dogs vs Cat dataset on the distracted drivers dataset.

### Input image sizes

The VGG16 model used 224 x 224 sized images as input. This confused me as to how was the Dogs vs Cats dataset, which had random sized images, trained on this model and how should I proceed with mine? It turns out that keras has the function `flow_from_directory()` which is used to read images from a specified directory. This functions also take a parameter `target_size` tuple, which allows us to specify the required image size and Keras takes care of resizing the input images.

### Predictions of all images in test set is the same class

After training my model for soem epochs, I was able to achieve a peak accuracy of ~70%. However, when I got to predicting the images in my test set, they all were being predicted as the same class! Turns out I was giving train and test images in different manner to the model to learn and predict. The model was trained with images with values on scale of 0-255, but I was predicting on test images on a scale of 0-1. A silly oversight!

### Using a "sample" directory really helps!

If all the dataset data is in `"data/"` directory, create a new folder `"data/sample"` with the same structure as the dataset. Have your `train`, `val` and `test` in the `sample` folder too, just with lesser number of images. this folder would really help in quick testing of the code.

So these are some ameteur mistakes I made, along with how to solve them. I hope these help you if you're ever stuck in the same spot as I was!

Comment below if you have any questions :)

