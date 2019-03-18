## Project: Build a Traffic Sign Recognition Program


# **Traffic Sign Recognition** 
---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./writeup/1.png "Visualization"
[image2]: ./writeup/2.png 
[image3]: ./writeup/3.png 
[image4]: ./writeup/4.png 
[image5]: ./writeup/5.png 
[image6]: ./writeup/6.png "Bar Chart"
[image7]: ./writeup/7.png "Visualization & Grayscaling"
[image8]: ./my_images/bumpy.jpg "Bumpy Road"
[image9]: ./my_images/caution.jpg "General Caution"
[image10]: ./my_images/no-vehicles.jpg "No Vehicles"
[image11]: ./my_images/stop.jpg "Stop"
[image12]: ./my_images/work.jpg "Road work"
[image13]: ./writeup/8.png 
[image14]: ./writeup/9.png 
[image15]: ./writeup/10.png 
[image16]: ./writeup/11.png 
[image17]: ./writeup/12.png 

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.


### Data Set Summary & Exploration

#### 1. Basic summary of the data set.

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32, 32, 3)
* The number of unique classes/labels in the data set is 43

#### 2. Visualization of the dataset.

Here is an exploratory visualization of the data set. Following shows some random images from the dataset

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]

Following is a bar chart showing sample distribution for 43 traffic signs

![alt text][image6]

### Designing and Testing a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

As a first step, I decided to convert the images to grayscale because color channels are not really required to train the model and they add extra confusion during training

Here is an example of a traffic sign image before and after grayscaling.

![alt text][image7]

As a last step, I normalized the image data because it will speed up the training by not having large deviations in dataset values


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model(LeNet) consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x1 grayscale image   					| 
| Convolution 3x3     	| 1x1 stride, valid padding, outputs 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x6 					|
| Convolution 3x3	    | 1x1 stride, valid padding, outputs 10x10x16	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 5x5x16 					|
| Flatten		      	| output 400									|
| Fully connected		| output 120   									|
| RELU					|												|
| Fully connected		| output 84   									|
| RELU					|												|
| Fully connected		| output 10   									|

 
#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used LeNet architecture. I used the AdamOptimizer with a learning rate of 0.001. The epochs used was 50 with the batch size also 50. This gave me an accuracy of 93%-95% on validation set and 91%-92% accuracy on test set.
 

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* validation set accuracy of 93%-95% 
* test set accuracy of 91%-92%

If a well known architecture was chosen:
* What architecture was chosen?
  * I used LeNet architecture as used in the lecture which initially gave me validation accuracy of around 89% but I increased this accuracy by tweaking epoch value to 50 and batch size to 50. The reason of increasing the epoch value to 50 was that with each epoch cycle, I was getting an improvement in the accuracy. Moreover, small batch sizes gave me higher accuracy so I chose the batch value of 50.
* Why did you believe it would be relevant to the traffic sign application?
  * While there can be other architectures which can be used and modified to train this model for which I ll keep studying and experimenting further but for the sake of this project, I chose to go with the architecture used in the lecture as I was already comfortable with its implementation.
* How does the final model's accuracy on the training, validation and test set provide evidence that the model is working well?
  * Accuracy of 92% on test set gave me the confidence to test this model on new images taken from web. 4 out of 5 such images were predicted correctly using this model which proved that this model is working model. Although I ll work further on improving this model to get better accuracy.
 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image8] ![alt text][image9] ![alt text][image10] 
![alt text][image11] ![alt text][image12]

I have used some easy to detect images after modifying there sizes to 32x32 pixels.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set 
Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Bumpy Road      		| Bumpy Road   									| 
| General caution       | Dangerous curve-right 						|
| No vehicles			| No vehicles									|
| Stop   	      		| Stop      					 				|
| Road work 			| Road work     				     			|


The model was able to correctly guess 4 of the 5 traffic signs, which gives an accuracy of 80%. This compares favorably to the accuracy on the test set of 92%

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)


![alt text][image13] ![alt text][image14] ![alt text][image15] 
![alt text][image16] ![alt text][image17]
