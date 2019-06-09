# **Behavioral Cloning** 

## Writeup / Readme


---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build a convolution neural network in Keras that predicts steering angles from images
* Train the model with a training set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: writeup_images/architecture.JPG "Architecture"
[image2]: writeup_images/center.jpg "Center"
[image3]: writeup_images/left.jpg "Left"
[image4]: writeup_images/right.jpg "Right"


## Rubric Points
#### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
### Files Submitted & Code Quality

#### 1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* train.ipynb containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* readme.md summarizing the results

#### 2. Submission includes functional code
Using the Udacity provided simulator and the drive.py file and my model.h5 file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

#### 3. Submission code is usable and readable

The train.ipynb file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

---

### Model Architecture and Training Strategy

#### 1. An appropriate model architecture has been employed

The CNN I used is the same architecture that the CNN in the paper "End to End Learning for Self-Driving Cars" by Nvidia. The only difference is, that the input image size is different, but the layers of the CNN are the same.

After a cropping and normalization layer, there are 5 convolution layers.
The first three of the conv layers use a 5x5 kernels and the following 2 conv layers use a 3x3 kernel.
Each convolution layer includes a subsampling by to. Alternatively, pooling could be used.
Afterwards 4 fully connected layers are connected.


The following architecture image shows the cropping layer and the first 3 convolution layers which are using a 5x5 kernel each: 

![alt text][image1]


To create the visualization of the CNN, I used an online tool which can be found on:

http://alexlenail.me/NN-SVG/LeNet.html


After appropriate training with enough training data, the CNN predicts a suitable steering angle and will keep the car on the track.





#### 2. Attempts to reduce overfitting in the model

The model contains one dropout layer after the five convolutional layers in order to reduce overfitting. 


I didn't split the data into a training and a validation set, because the validation loss is not a good indicator how well the car drives. Therefore, all data was used for training and after training the model was tested directly how well it works in the simulator. 


Further, overfitting was reduced by using a high number of training images and by using data augmentation which is described in the next section.


#### 3. Appropriate training data

The training data is quite essential for this behavioral cloning task. I used in total 73k images for the training that were recorded during 4 runs. 3 runs are in the normal direction, one run was done in the opposite direction.


The center image is quite useful to train the model to stay in the center if the car is located in the center. 

An example of a center image:

![alt text][image2]


If the car comes a little bit off the center, the car is in a state which is not part of the center training data, why the car could completely loose the track.

Therefore, I used not only the center image, but also the left and right camera image and applied the correction term on it. The left (right) image is quite similar to a scene where the car drives too far to the left (right) and with the correction term the car learns to steer to the right (left) if it is located too far left (right). 

A left and a right image:

![alt text][image3]
![alt text][image4]

The left and right images are really useful, because they can train the model what to do if it leaves the center of the lane. 

Getting more training data by flipping all the images horizontally is a good method, but was not used in the project because I wasn´t able to flip the steering angle accordingly in the image data generator.


For the training itself I used the Keras method flow_from_dataframe which is quite useful to train with many images and avoids that all images must be stored in the RAM of the computer during training.


#### 4. Model parameter tuning

The used model parameters are straight forward. As steering correction, the 0.2 suggested in the Udacity video was used unchanged. I adapted the learning rate of the Adam optimizer to 0.0001 to ensure that the learning isn´t too fast. Because the CNN is relatively small, it is not a big issue to have a small learning rate in terms of training duration. 



---

### Results

Using the model.h5 file for the simulator in autonomous mode gives out a quite smooth driving experience. 
The video which was recorded during the autonomous round on the track is the file run1.mp4.

---
How the model can be used to drive the car in autonomous mode can be read in the readme file of Udacity:
https://github.com/udacity/CarND-Behavioral-Cloning-P3/blob/master/README.md

