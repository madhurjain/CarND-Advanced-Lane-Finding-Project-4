## Advanced Lane Finding Project
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

Overview
---
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

[image1]: ./camera_cal/calibration18.jpg "Camera Calibration Image"
[image2]: ./output_images/marked_calibration18.jpg "Marked"
[image3]: ./output_images/undistorted_calibration18.jpg "Undistorted"
[image4]: ./test_images/test2.jpg "Road Image"
[image5]: ./intermediate_images/test2_undistorted.jpg "Undistorted Road Image"
[image6]: ./intermediate_images/test2_binary.jpg "Binary Curved Lane Lines"
[image7]: ./intermediate_images/straight_lines1_warped.jpg "Warped Straight Lane Lines"
[image8]: ./intermediate_images/test2_centroids.jpg "Warped Pixel Centroids"
[image9]: ./intermediate_images/test2_lines.jpg "Line Fit"
[image10]: ./output_images/test2.jpg "Output"
[video1]: ./project_video.mp4 "Video"

---

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

> The code for this step is contained in the second code cell of the IPython notebook "P4.ipynb".

> I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

> I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

> ![alt text][image1]
> ![alt text][image2]
> ![alt text][image3]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

> To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
> ![alt text][image4]
> ![alt text][image5]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

> I used color selection to generate a binary image. The code for this steps is contained at the bottom of fourth code cell of my IPython notebook. Here's an example of my output for this step.

> ![alt text][image6]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

> The code for my perspective transform includes a function called `warp()`, which appears in the fifth code cell of the IPython notebook `P4.ipynb`.  The `warp()` function takes as inputs an image (`img`), as well as  the `camera_matrix` obtained using the function `get_camera_matrix()`. I chose to hardcode the below source and destination points :

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 222, 700      | 395, 700      | 
| 582, 456      | 395, 0        |
| 700, 456      | 884, 0        |
| 1082, 700     | 884, 700      |

> I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

> ![alt text][image7]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

> Then I identified the lane line pixel centroids in the fifth code cell of the IPython notebook in function `find_window_centroids()`:

> ![alt text][image8]

> Then using the centroids found, I fit my lane lines with a 2nd order polynomial kinda like this:

> ![alt text][image9]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

> I did this in the fifth code cell of the IPython notebook in the function `get_lane_curvature()`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

> I implemented this step in the fifth code cell of the IPython notebook in the function `draw_overlay()`.  Here is an example of my result on a test image:

> ![alt text][image10]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

> Here's a [link to my video result](./output_videos/project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

> The pipeline seems to be working great for the project video but some improvement needs to be done to get it working for the other videos. Especially, the lightning conditions affect the lane line detection which disturbs the next steps of the pipeline. Fine tuning the thresholding will help get better results. 
