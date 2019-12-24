## Assessment

#### Problem 1: Given a 256x256 image, write a binary classifier to differentiate the given image has a human or not. The approach and the code are more important than the accuracy rate. Please feel free to pick a relevant open dataset for this problem.

I have made used of Transfer Learning using the VGG16 model for the purpose of this classification. The implementation has been done using Keras. 

Dataset: It consists of two folders:
Folder1: Human Images
Folder2: Images of Dogs, Cats and Horses

<img src="/imgs/data.jpg" width="250" height="150">

Train Data: 360 images belonging to 2 classes.
Test Data: 24 images belonging to 2 classes.

Step1:
Load the VGG16 model and its weights when it was trained on the imagenet dataset and specify your own input shape (256x256).

Step2:
We will need to fine tune the last few blocks of the model to cater to our problem. The orignal model has the last Max Pooliing layer of 1000 units which needs to be replaced by 2 in our case (binary classification). We modify the last few layers in the following manner:

<img src="/imgs/vgg16_1.jpg" width="500" height="200"> <img src="/imgs/vgg16_2.jpg" width="300" height="200">

We use global average pooling (GAP) layers to minimize overfitting by reducing the total number of parameters in the model. However, GAP layers perform a more extreme type of dimensionality reduction, where a tensor with dimensions h×w×d is reduced in size to have dimensions 1×1×d. GAP layers reduce each h×w feature map to a single number by simply taking the average of all hw values.

Step3:
We then freeze the layers of the VGG16 model that were unchanged as they do not need to trained and we could use the weights obtained from the ImageNet datasets for this classification problem as well.

Step4: 
Categorical Cross Entropy (log loss) is used as the problem has categorical data and cross entropy is used in when the model has to output between 0 and 1. A perfect model has a log loss of 0. Adam is used as the optimizer to compile the model.

<img src="/imgs/log_loss.jpg" width="200" height="200">

Step5:
The model is then trained using fit_generator and the test accuracy is obtained using evaluate_generator.

#### Problem 2: Given an image, identify the dominant colors (hex code) eliminating the neutral colors (white, black and greys) using OpenCV.

I have made use of K-Means Clustering to solve this problem.

Step1:
The images in loaded in opencv are in BGR format, the first thing done was to convert in to RGB. The parameters we have are 3D and we won't be needing rows and columns separately so it was merged and converted to a 2D matrix.

Step2:
Use the K-Means classifier and fit it to the image.

Step3:
Get a histogram for the clusters and then find the ratio/percentage probability of each cluster dominating the image.

Step4:
Choose the cluster of the centroid with maximum probability and obtain it's RGB value.

Step5:
Using thresholds, increase the value of the channel with maximum value and decrease the value of the channel with lowest value to get closer to the actual dominant color in the image. 

Step6: 
Convert the modified RGB to hex code and display it along with the color.

###### Note: The number of clusters plays a crucial role in the solution. We can use R to load the data and using either Gap Statistic or Elbow method to find the optimal number of clusters for the given data.
Ex: If the image of a green park with clear blue sky has two clusters, the output would be green as the dominant color.
But if the number of clusters are 3, it would treat the shades of green as two different colors and blue would be the dominant color.

<img src="/imgs/park_1.jpg" width="400" height="400"> <img src="/imgs/park_2.jpg" width="400" height="400">

