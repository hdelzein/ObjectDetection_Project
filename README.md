# Object Detection in an Urban Environment

## Project Overview

This is SSD (Single Shot Detector) model training project. The goal is to classify objects of cars, pedestrians and 
cyclists as a classfication problem and to determine the location of these objects or their bounding boxs in the images
as a regression problem. 

For this project, we will be using data from the [Waymo Open dataset](https://waymo.com/open/) and a pretrained model 
which is the ssd_resnet50_v1_fpn_640x640_coco17_tpu-8.tar.gz. We shall use Tensor flow object detetction API to custimize
the training of the model. The Tensor Object Detection API relies on a config file to custimize the model training.

## Project Setup

For this project I used the Udacity Desktop ennviroment fot training and evaluation. The structure of 
the data on Udacity Desktop enviroment is as follow:

### Data Structure
The ia structure of the data used for training, validation and testing is organized as follow on the Udacity Desktop enviroment:
```
/home/workspace/data/waymo
    - training_and_validation - contains 97 files to train and validate your models
    - train: contain the train data (empty to start)
    - val: contain the val data (empty to start)
    - test - contains 3 files to test your model and create inference videos
```
### Experiments Folder

This is the default experiments folder structure provided by Udacity on the Desktop enviroment:
```
experiments/
    - pretrained_model/
    - exporter_main_v2.py - to create an inference model
    - model_main_tf2.py - to launch training
    - reference/ - reference training with the unchanged config file
    - experiment0/ - create a new folder for each experiment you run
    - label_map.pbtxt
    ...
```

### New Folder on the GitHub Enviroment

I created a Final_Config_File folder in GitHub that include the final version of the pipeline_new.config
file that I used to train and evaluate the final model.

```
Final_Config_File/
    - pretrained_model/pipeline_new.config

```

I created three directories that hold the Data Analysis Images, Data Augmentation Images and the Tensorboards Images.

```
DataImages/
augImages/
TensorBoardFigures/

```



## Data Set

### Data Exploratory Analysis 

I used the Exploratory Data Analysis.ipynb to plot ten random figures with cars bounding boxes in blue and
pedesrians bounding boxes in green and cyclists bounding boxes in yellow. The cars were the most extremly 
frequent objects followed by pedestrians. Cyclists were very rare. As the images below shows that the data 
included images with different brightness, and intensity levels. Also the plots included images of objects 
at different, depth, scale, and with occulsions. More data analysis of the frequency of objects and their 
distributions over the image dataset could have been very usefull and interesting if time on the GPU permitted.


<img src="DataImages/one_one.png" width="1000">
<img src="DataImages/two_one.png" width="1000">
<img src="DataImages/three_one.png" width="1000">
<img src="DataImages/four_one.png" width="1000">
<img src="DataImages/five_one.png" width="1000">
<img src="DataImages/six_one.png" width="1000">
<img src="DataImages/seven_one.png" width="1000">
<img src="DataImages/eight_one.png" width="1000">
<img src="DataImages/nine_one.png" width="1000">
<img src="DataImages/ten_one.png" width="1000">

### Data Augmentation Analysis

The execution of the Explore augmentations.ipynb notebook showed two images inside of the notebook. I modified the notebook 
and the config file to plot four augmented images. These images from the left to the right is "Horizantol Flip", "Crop Image", 
"Adjust Brightness", and "Adjust Contrast". Something that would have been very usefull is the ability to plot the original 
image next to its augmented image to visualize the effect that is made on the images.

<img src="augImages/flipHorizantal.png" width="150" title = "Horizantol Flip"> <img src="augImages/cropImage.png" width="150" title = "Crop Image">
<img src="augImages/adjustBright.png" width="150" title = "Adjust Brightness"> <img src="augImages/adjustContrast.png" width="150" title = "Adjust Contrast">

## Training

### Training Experiment

The training was executed on a batch size of 2 for total number of steps of 2500 with a low learning rate of 0.0006. As a 
result we can see that the losses is not varying smoothly from one period to the other because of the small batch size. That is 
because that one of the small batch size might generates a low loss but suddenly the performance worsen on the next batch size. 
Hence at the end of the total steps, the total loss can be either high or low based on the randomness of which batch of images 
are tested toward the end. Hence it is better to consider the smooth total loss versus the value of the loss at the last step. 
For the total loss below, the training loss was equal to 0.6407 while the evaluation loss was equal to 0.877. I run this 
experiment multiple times and I realized that even when the training can vary from one run to the next and it can overfit or not
ovberfit, the evaluation loss value varied between 0.8 to 0.9. Also since the visalization of the evaluation is a dot versus a
curve it was hard to examine how evaluation loss is trending.

As far as the as the classification loss, the training and evaluation losses were low and close to each other. On the other hand 
localiztion training and evaluation losses were close to each others but they were very slightly higher than the classification 
losses. That indicate that the algorithm did a better job classifying objecys versus localizing the objects. A good experiment 
could have been the ability to determine which images was misclassified or misslocalized and to analyze the data and maybe
feed more training examples into the system to train it better. 

The regulariztion loss was decreasing through the run, but it flattened toward the end which could means that the system overfitted
slightly in this particular execution of the experiment.

I would have liked to run this experiment with a higher number of batchs like 10 and with around total number of steps between 2500 
and 4000 but the memory limitation and GPU time limitation did not allow for that. This experiment I believe could have generalized
better on the evaluation step.

Access to inspect the images that were missclassified during training and evaluation would have been very usefull for better evalauting 
precision and recall.  

This test used two augmentation techniques which are horizantal flip and image crop. If time permitted I would have likded to add random
brightness, random contrast, random hue and distort color.

<img src="TensorBoardFigures/totalLoss.jpg" width="250"/> <img src="TensorBoardFigures/learning rate.jpg" width="250"/>
<img src="TensorBoardFigures/class_local_loss.jpg" width="500">
<img src="TensorBoardFigures/norm_reg_loss.jpg" width="500">
<img src="TensorBoardFigures/detectPrecisionOne.jpg" width="500">
<img src="TensorBoardFigures/detectPrecisionTwo.jpg" width="500">
<img src="TensorBoardFigures/detectPrecisionThree.jpg" width="500">
<img src="TensorBoardFigures/recallOne.jpg" width="500">
<img src="TensorBoardFigures/recallTwo.jpg" width="500">
<img src="TensorBoardFigures/recallThree.jpg" width="500">


### Improve the reference

I would have liked to run the experiement with the following modifications:

1) Large number of bacth size and total number of steps
2) Different optimizing algorithm such as Adams
3) Differen machine learning rate annealing techniques


## Video Animation

A video inference was generated at the end of this project and a file named animation.gif was geneated successful that showed
good performance. The fils is saved in the following directory:

```

ObjectDetection_Project/animation.gif

```