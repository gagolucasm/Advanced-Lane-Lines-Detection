# Advanced Lane Finding Project
## Writeup 
Lucas Gago
SDCND P4
3/13/17

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

[image1]: ./images/color_filters.png "colorfilt"
[image2]: ./images/other_filters.png "other"
[image3]: ./images/example_calibration.png "examples"
[image4]: ./images/hist.png "hist"
[image5]: ./images/color_fit_lines.jpg "Fit Visual"
[image6]: ./images/out.png "Output"
[image7]: ./images/persp_transform.png "persp"
[image8]: ./images/line_fit.png "linefit"
[video1]: ./out_project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. 
You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./examples/example.ipynb"

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the tests images using the `cv2.undistort()` function and obtained the results presented into `Advanced_lane.ipynb`.





### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

![alt text][image3]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.
I used a combination of color and gradient thresholds to generate a binary image. The code is contained into `hls_color_thresh` , `sobel_x`, `mag_thresh`, `dir_threshold` and `mag_dir_thresh` functions. Examples are provided in the notebook.
![alt text][image1]
![alt text][image2]
#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

Inside the section Perspective Transformation, I choose the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 203, 720      | 320, 720      |
| 1127, 720     | 960, 720      |
| 695, 460      | 960, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

I used `cv2.getPerspectiveTransform` function to get the perspective transform.

![alt text][image7]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I did this in function `fitlines` in my code in `Advanced_Line.ipynb`, using `np.polyfit(lefty, leftx, 2)`, like this:

![alt text][image5]

Results examples are:

![alt text][image8]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in function `curvature` in my code in `Advanced_Line.ipynb`. I used the ecuation of the radious of curvature provided [here](http://www.intmath.com/applications-differentiation/8-radius-curvature.php). 

As the camera is mounted at the center of the car, the lane center is the midpoint at the bottom of the image between the two lines detected. The offset of the lane center from the center of the image (converted from pixels to meters) is the distance from the center of the lane.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link](./out_project_video.mp4) to my video result

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

No problems in the implementation, but tunning manually color parameters for filters seems to be a bad technique, because light conditions as well as road changes could make the system unstable. Maybe using wide ranges of color between max and min and tunning the system to work with it could be an improvement.

