
**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Implement a sliding-window technique and use the trained classifier to search for vehicles in images.
* Create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./examples/car_not_car.png

[pipelineResult]: ./output_images/VehicleDetectionPipelineResult.PNG

[video1]: ./project_video_output.mp4



I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]


I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`). 

I tried various combinations of parameters and figured that the 'YCrCb' color space worked the best along with 9 orientations, 8 pixels per cell, and 2 cells per block values.

I trained a linear SVM using data formed of spatial color data, color histogram data, and hog features stacked together in a long single dimension vector. (function name = extract_features). I normalized this as well.

I decided to search for window size of 64 pixels over the image (400< y < 656). I did not need to use changing scales for the windows. I did use the heatmapping technique to reduce the outliars and make proper rectangles around the cars.

Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result. I checked different color schemes and found that YCrCb worked the best.  Here are some example images:

![alt text][pipelineResult]
---

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  
I showed some examples above.

---

Some issue is that the cars are not identified for some of the frames. One thing to do here would be to implement an advanced version of the sliding window technique and check for variable sizes. 
