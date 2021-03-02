# **Behavioral Cloning** 



[//]: # (Image References)

[image1]: ./examples/1.png "Dataset Distribution"
[image2]: ./examples/2.png "Dataset Distribution After Adding Multiple Sensor Data"
[image3]: ./examples/before_translation.png "Original Image"
[image4]: ./examples/after_translation.png "Warped Image"
[image5]: ./examples/cropped.png "Cropped Image"
[image6]: ./examples/flipped_Cropped.png "Flipped Image"
[image7]: ./examples/hsv_random_noise.png "Flipped Image"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
### Files Submitted & Code Quality

#### 1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup_report.md or writeup_report.pdf summarizing the results

#### 2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

#### 3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

### Model Architecture and Training Strategy
The architecture is introduced in architecture() function. 

#### 1. An appropriate model architecture has been employed

My model consists of a convolution neural network. Overall architecture is influenced from "End to End Learning for Self-Driving Cars, NVIDIA" paper. 

Convolution layers have different kernel sizes: Three of them is 5x5 and 2 of them is 3x3. Number of filters are gradually increasing: 24->36->48->64->64

ELU function adds nonlinearity into the model. This function uses after Conv Layers.

Data is normalized before feed to model. 

#### 2. Attempts to reduce overfitting in the model

The model contains dropout layers in order to reduce overfitting. 

The model was trained and validated on given data sets to ensure that the model was not overfitting. This train/valid division is provided by train_test_split() function from sklearn library and the rate is 0.2. In other words, 20% of the data is used as validation. The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track. (Please see run1.mp4)

#### 3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually.

#### 4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. I used a combination of center lane driving, recovering from the left and right sides of the road. The data generator chooses randomly from center, right and left cameras.


### Model Architecture and Training Strategy

#### 1. Solution Design Approach

My first step was to search for a model for this purposes. I found NVIDIA's paper. With the help of open sources, I could accomplish.  

In order to gauge how well the model was working, Isplit my image and steering angle data into a training and validation set. This approach is necessary to understand the model's behavior whether it is underfitting or overfitting. Although I added dropout layers to combat overfitting, validation is where we can see dropout layers work.

The final step was to run the simulator to see how well the car was driving around track one. There were a few spots where the vehicle fell off the track. So, I increases the epoch number to learn well.

At the end of the process, the vehicle is able to drive autonomously around the track without leaving the road.

#### 2. Final Model Architecture

The final model architecture (model.py architecture() function) consisted of a convolution neural network with the following layers and layer sizes:

| Layer         	|     Description	        		| 
|:--------------------:|:--------------------------------------------:| 
| Input         	| 66x200x3 Normalized  			| 
| Convolution 5x5     	| 2x2 stride, valid padding, 24 Filters	|
| ELU			|						|
| Dropout	      	| 0.2 probability 				|
| Convolution 5x5     	| 2x2 stride, valid padding, 36 Filters	|
| ELU			|						|
| Dropout	      	| 0.2 probability 				|
| Convolution 5x5     	| 2x2 stride, valid padding, 48 Filters	|
| ELU			|						|
| Dropout	      	| 0.2 probability 				|
| Convolution 3x3     	| 1x1 stride, valid padding, 64 Filters	|
| ELU			|						|
| Dropout	      	| 0.2 probability 				|
| Convolution 3x3     	| 1x1 stride, valid padding, 64 Filters	|
| ELU			|						|
| Dropout	      	| 0.2 probability 				|
| Fully connected	| 100 units					|
| ELU			|						|
| Dropout	      	| 0.2 probability 				|
| Fully connected	| 50 units					|
| ELU			|						|
| Dropout	      	| 0.2 probability 				|
| Fully connected	| 10 units					|
| ELU			|						|
| Fully connected	| 1 units					|


#### 3. Creation of the Training Set & Training Process

To capture good driving behavior, I analyzed the given dataset. The image shows the distribution of a given dataset. 

![alt text][image1]

Then, I decided to take into account right and left images. I applied similar process into the course. However, instead of adding +-0.2 steering angle, I introduced 0.25 steering angle. After this method, the distribution changes:

![alt text][image2]

The distribution become better but still was not fullfilling. So, I decided to record from Track2. However, just using given dataset gives good result surprisingly, so I do not need to use them.
In addition to increasing number of data, I applied preprocessing for robusteness.First, I warped images. I did it because people mentioned this to compansate for steering angle and they benefited. Before and after is shown here:  

![alt text][image3]
![alt text][image4]

Then, I cropped images to focus on Region of interest.

![alt text][image5]

Also, I flipped images and angles randomly thinking that this would increase distribution. For example, here is an image that has then been flipped after cropped:

![alt text][image6]

To make data more resistant to light conditions, I played with color spaces: HSV. I added random brightness into image by adding random white noise into V channel.

![alt text][image7]


I finally randomly shuffled the data set and put 20% of the data into a validation set. 

I used this training data for training the model. The validation set helped determine if the model was over or under fitting. 24 epoch was enough. I controlled with one of the Tensorflow modeule Checkpoint:Early_Stopping. I used an adam optimizer so that manually training the learning rate wasn't necessary.
