# Advanced-Lane-Finding
Built an advanced lane-finding algorithm using distortion correction, image rectification, color transforms, and gradient thresholding. Identified lane curvature and vehicle displacement. Overcame environmental challenges such as shadows and pavement changes.

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./1.png "Calibration Photos"
[image2]: ./2.png "Undistorted"
[image3]: ./3.png "Undistorted Photo"
[image4]: ./4.png "Warp Example"
[image5]: ./5.png "Binary"
[image6]: ./9.png "Output"
[image7]: ./10.png "Output1"
[video1]: ./project_video_output.mp4 "Video1"
[video2]: ./challenge_video_output.mp4 "Video2"

---
### Camera Calibration

#### Camera Matrix and Distortion Coefficient Computations

The code for this step is contained in the second, third, fourth code cells of the IPython notebook located in "pre.ipynb".  
Most of the code used here in this process is from the Udacity lessons. The images are read with mpimg.imread so cv2.COLOR_RGB2GRAY is used to convert the images to gray scale. cv2.findChessboardCorners is followed after to collect objpoints and imgpoints. By using pickle, mtx and dist are collected for each calibration image that was ran through undistort function.

![alt text][image1]
![alt text][image2]

### Pipeline (single images)

#### Example of a Distortion-Corrected Image

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image3]

#### Color Transforms and Gradients for Thresholded Binary Image

I used a combination of color and gradient thresholds to generate a binary image (code cells eight, nine, and ten in `pre.ipynb`).  Here's an example of my output for this step.  (test image 2)

![alt text][image5]

#### Perspective Transform

The code for my perspective transform includes a function called `transform_image()` in code cell six in pre.ipynb.  The `transform_image()` function takes as inputs an image (`img`), image size (size) as well as source (`src`) and destination (`dst`) points. Code cell seven explains how src and dst are implemented.

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### Lane-Line Pixels Identification

In code cells 14, 15, and 16 in pre.ipynb, lane-line pixels are calculated (from scipy.signal import find_peaks_cwt). Here they are then separated from left lane to right. In code cell three from post.ipynb sliding window search is used to find peaks and connect afterwards.

![alt text][image6]

#### Vehicle Position Calculation

I mostly borrowed Udacity's code in lesson 35 Measuring Curvature. This is located in code cell three in post.ipynb. Functions such as curve_form is used to calculate the curve formula.

#### Example Image of Result

The lane maskings and original image are combined using cv2.bitwise_or, which can be found in code cell five in post.ipynb.

![alt text][image7]

---

### Pipeline (video)

#### Results
##### Basic
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/xgv6zKWNXYM/0.jpg)](https://www.youtube.com/watch?v=xgv6zKWNXYM)
##### Advanced
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/ng9edgddoms/0.jpg)](https://www.youtube.com/watch?v=ng9edgddoms)
---

### Discussion

The pipeline used here does not work too well with fast changes of curves in the road. Whenever this pipeline encounters peaks that are different in a certain magnitude, it considers them as outliers and ignores those peaks and uses past frames' data to complete the curve. This is why if the road curve changes direction very fast, the pipeline requires a bit of time to adjust to the related curve direction. To fix this, value to control the pipeline's outlier factor may be improved. Also the method of what to do with outliers may be improved in such a way that when the pipeline encounters outliers, the pipeline 'foresees' the direction of the curve and places 'expected' points in frames with outliers. Lastly, lane line detections may be improved with more filter adjustments to eliminate outliers entirely.
