# Writeup

**Advanced Lane Finding Project**

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

[video1]: ./project_video.mp4 "Video"
---
### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the second, third, fourth code cells of the IPython notebook located in "pre.ipynb".  
Most of the code used here in this process is from the Udacity lessons. The images are read with mpimg.imread so cv2.COLOR_RGB2GRAY is used to convert the images to gray scale. cv2.findChessboardCorners is followed after to collect objpoints and imgpoints. By using pickle, mtx and dist are collected for each calibration image that was ran through undistort function.

![alt text][image1]
![alt text][image2]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image3]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (code cells eight, nine, and ten in `pre.ipynb`).  Here's an example of my output for this step.  (test image 2)

![alt text][image5]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `transform_image()` in code cell six in pre.ipynb.  The `transform_image()` function takes as inputs an image (`img`), image size (size) as well as source (`src`) and destination (`dst`) points. Code cell seven explains how src and dst are implemented.

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

In code cells 14, 15, and 16 in pre.ipynb, lane-line pixels are calculated (from scipy.signal import find_peaks_cwt). Here they are then separated from left lane to right. In code cell three from post.ipynb sliding window search is used to find peaks and connect afterwards.

![alt text][image6]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I mostly borrowed Udacity's code in lesson 35 Measuring Curvature. This is located in code cell three in post.ipynb. Functions such as curve_form is used to calculate the curve formula.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

The lane maskings and original image are combined using cv2.bitwise_or, which can be found in code cell five in post.ipynb.

![alt text][image7]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)
Here's the challenge video [link to my video result](./challenge_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The pipeline used here does not work too well with fast changes of curves in the road. Whenever this pipeline encounters peaks that are different in a certain magnitude, it considers them as outliers and ignores those peaks and uses past frames' data to complete the curve. This is why if the road curve changes direction very fast, the pipeline requires a bit of time to adjust to the related curve direction. To fix this, value to control the pipeline's outlier factor may be improved. Also the method of what to do with outliers may be improved in such a way that when the pipeline encounters outliers, the pipeline 'foresees' the direction of the curve and places 'expected' points in frames with outliers. Lastly, lane line detections may be improved with more filter adjustments to eliminate outliers entirely.