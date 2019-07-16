# Vehicle Detection Project

* I Performed a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Implemented a sliding-window technique and use your trained classifier to search for vehicles in images.
* Ran the pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimated a bounding box for vehicles detected.


### Histogram of Oriented Gradients (HOG)

The code for this step is contained in the 2-11 code cells of the Experiment_Vehicle_Detection IPython notebook 

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![Example car and not car](http://i.imgur.com/KcuqkIe.png)

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:

![features](http://i.imgur.com/nw9m3Z6.png)


I tried various combinations of parameters and decided that the `YCrCb`
color space gave the best results.

In the 15-16 code cells in the Experiment_Vehicle_Detection IPython notebook I trained a linear SVM. The training time was 43.35 sec and the accuracy was 0.9901. It took 0.02931 Seconds to predict 10 labels with the SVC.

### Sliding Window Search

I decided to search random window positions at random scales all over the image and came up with this (ok just kidding I didn't actually ;):

![alt text](http://i.imgur.com/UhO3O2K.jpg)

I used the "find cars" method from class, with scale of 1.5.The search was limited by adding y start and y stop of the search area in the image.
Instead of overlap, there were 2 cells to step.

Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

![windows](http://i.imgur.com/4R0hhQp.png)
---

### Video Implementation

[![Project Output](http://i.imgur.com/2vQZkgJ.png)](https://vimeo.com/229826782 "Project Output")

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are five frames and their corresponding heatmaps:

![heatmaps](http://i.imgur.com/oBaPFrW.jpg)

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
![labels](http://i.imgur.com/esYTUpl.png)

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![last frame](http://i.imgur.com/P07I8M3.png)


---

### Discussion
 
I think that there might be a problem, if the car is only partially visible, a humane would be able to detect that this is a car but the classifer might not, and this can be potentially dangerous.
One solution might be to constantly update and train the classfier with partial image cars. 
An addition solution is to use additional technologies to identify the cars at run time, such as more sensors.


