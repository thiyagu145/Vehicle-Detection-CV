# Vehicle-Detection-CV
This is the fifth project in Udacity Self-Driving car nanodegree. The main objective of this project is to detect the vehicles in a video stream and track them throughout the video with the use of Computer Vision techniques. The project pipeline is contained in the jupyter notebook file in the repository. The main steps used for the project are mentioned below:
1. Extracting the Histogram of Gradient(HOG) features from the given set of training images
2. Training a classifier using the HOG feature vectors
3. Extracting the HOG features using different windows on the target image
4. Classifying the sub-images using the trained classifier and detect the bounding boxes
5. Use the bounding boxes and find the heat map to track the car in consequent frames
6. Based on the heat map, find the best bounding box and impose it onto the original image

## Preparing the Dataset
The dataset comprises of the GTTI dataset and the KITTI dataset. The datasets are loaded along with their labels (1 for car images and 0 for non-car images). A sample set of images are given below:
![alt text](https://github.com/thiyagu145/Vehicle-Detection-CV/blob/master/output_images/Screen%20Shot%202018-08-04%20at%205.36.40%20PM.png)

## Extracting the HOG features
The HOG features from the images were extracted using the function **get_hog_features()** which uses the OpenCV functions for HOG feature extraction. This functions requires a number of parameters such as **pixels_per_cell()**, **cells_per_block()** and **orientation()**. Orientation decides number of gradient bins we want the angles to be distributed in, the pixels per cell decides the number of cells over which the gradients are calculated. The cells per block specifies the local area over which the gradients are calculated. Different combinations of these parameters were tested and the best set of parameters were chosen based on the performance of the classifier. The best parameters are given below: 
1. Cells per block: 2
2. Pixels per cell: 16
3. Orientations: 11 </br>
A sample set of images along with their HOG feature images are given below:
![alt text](https://github.com/thiyagu145/Vehicle-Detection-CV/blob/master/output_images/Screen%20Shot%202018-08-04%20at%205.37.30%20PM.png)

## Training the classifier
Once the HOG features are extracted, the classifier is trained to classify the image as car or non-car based on the HOG features. In this case, an SVM with a linear kernel was trained. The parameters used for the classifier were the default parameters. Test accuracy of 98.3% was obtained with this classifier. The feature vectors were not scaled since only the HOG features were used for training the classifier. If other features such as the spatial or the color histograms were used, then the feature vector has to be normalized. 

## Sliding window based search
After the classifier is trained, the HOG features from the test images have to be obtained on a sliding window basis. This is performed in the function **find_cars()**. We have to decide the size of the window and the scale. We can also crop the image to perform the window search in a specified region of the image. Finding the HOG features for all the windows again and again is a time consuming process and hence the HOG features are found out for the entire image and they are sub-sampled based on the window regions and this is called as the Sliding window search. Also, the scale of the windows are varied and in this case, three scale values (1.5, 2.5, 3.5) are used. Once this search is performed, bounding boxes are obtained. Result of Sliding window search on the test images are given below:
![alt text](https://github.com/thiyagu145/Vehicle-Detection-CV/blob/master/output_images/Screen%20Shot%202018-08-04%20at%205.42.15%20PM.png)

## Finding cars using heatmap
Bounding boxes will be obtained for every region of the car and hence there will be more than one bounding box per car depending on the size of the car. To overcome this issue and get only one bounding box per car, heatmaps can be used. The pixels inside each of the bounding boxes are increased by value 1 based on the number of boxes they are present in. Once the heatmaps are obtained, a single bounding box can be obtained by finding the centroid of these heatmaps. Heatmaps of few of the test images are given below:
![alt text](https://github.com/thiyagu145/Vehicle-Detection-CV/blob/master/output_images/Screen%20Shot%202018-08-04%20at%205.42.42%20PM.png)

## Framing the pipeline for video processing
Once the algorithm performs well on the testing images, the next step is to frame the pipeline for video processing. To obtain stable bounding boxes, a class for storing the bounding box is created and the last 15 lists of bounding boxes are stored. This is highly required to prevent false positives from appearing as a bounding box in the frame. The speed of the algorithm is approximately 10-15 frames per second. The pipeline is applied to the given project video and the output is stored as **final_output.mp4** in the following link ![final output](https://github.com/thiyagu145/Vehicle-Detection-CV/blob/master/final_output.mp4)

## Discussion 
From the video output of the file, we can see that the vehicles are detected properly except for few false positives. Another problem is that, due to heat maps when 2 cars are present very close to each other they are considered in a single bounding box. To overcome this issue, the scale can be varied and made smaller to detect smaller bounding boxes which might help prevent bounding boxes of 2 different cars to get merged. Apart from this, the algorithm performs pretty well on most of the frames. Deep Learning techniques can be used to improve the detections further. 



