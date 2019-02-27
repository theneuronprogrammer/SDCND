## Project Writeup


---

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

[image1]: ./output_images/undistorted1.png "Undistorted1"
[image2]: ./output_images/undistorted2.png "Undistorted2"
[image3]: ./output_images/undistorted3.png "Undistorted3"
[image4]: ./output_images/undistorted4.png "Undistorted4"
[image5]: ./output_images/undistorted5.png "Undistorted5"
[image6]: ./output_images/undistorted6.png "Undistorted6"
[image7]: ./output_images/undistorted7.png "Undistorted7"
[image8]: ./output_images/undistorted8.png "Undistorted8"


[image9]: ./test_images/test1.jpg "Test1"

[image10]: ./output_images/threshold1.png "Threshold1"
[image11]: ./output_images/threshold2.png "Threshold2"
[image12]: ./output_images/threshold3.png "Threshold3"


[image13]: ./output_images/transformed.png "Transform1"
[image14]: ./output_images/transformed2.png "Transform2"
[image15]: ./output_images/transformed3.png "Transform3"


[image16]: ./output_images/linefit1.png "Linefit1"
[image17]: ./output_images/linefit2.png "Linefit2"


[image18]: ./output_images/final1.png "Final1"
[image19]: ./output_images/final2.png "Final2"
[image20]: ./output_images/final3.png "Final3"
[image21]: ./output_images/final4.png "Final4"
[image22]: ./output_images/final5.png "Final5"
[image23]: ./output_images/final6.png "Final6"


[image24]: ./output_images/tf_binary1.png "Binary1"
[image25]: ./output_images/tf_binary2.png "Binary2"
[image26]: ./output_images/tf_binary3.png "Binary3"


[image27]: ./output_images/lane_line1.png "LaneLine1"
[image28]: ./output_images/lane_line2.png "LaneLine2"
[image29]: ./output_images/lane_line3.png "LaneLine3"



[video1]: ./project_video.mp4 "Video"
[video2]: ./result.mp4 "Video"



## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README



### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./examples/example.ipynb" (or in lines # through # of the file called `AdvancedLaneFinding_p2.py`).  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image. Thus, objp is just a replicated array of coordinates, and objpoints will be appended with a copy of it every time I successfully detect all chessboard corners in a test image. imgpoints will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection. I start with 9 corners in x-axis and 6 corners in y-axis. However, some of the calibration images only show a portion of the whole image and the we couldn't find 9 and 6 corners from them. As a result, I add a logic to search for fewer corners (e.g., 8 and 6, 9 and 5, etc.) once we couldn't detect corners from the original numbers. In this way, I'm able to find all the corners from the calibration images and able to use all of them for calibration.

I then used the output objpoints and imgpoints to compute the camera calibration and distortion coefficients using the cv2.calibrateCamera() function. I applied this distortion correction to the test image using the cv2.undistort() function and obtained this result:

![alt text][image1]
![alt text][image2]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:

![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]
![alt text][image7]
![alt text][image8]

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image9]



#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image  The thresholds include:

 1. `Sobel` on `x` direction
 2. `Yellow` and `white` color detection from `RGB`
 3. `Yellow` and `white` color detection from `HSV`
 4. `Yellow` and `white` color detection from `HLS`
 5. `S channel` from `HLS`

The numbers for the thresholds are all hard-coded for now. A more robust approach will be explored later. Here's an example of my output for this step of the test image:

![alt text][image10]
![alt text][image11]
![alt text][image12]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

 The `PerspectiveTransform()` function uses hard-coded pixel locations for both the original and new images and calculates the `M` and `Minv` matrices for perspective transform. Then, the `M` matrix is used by `cv2.warpPerspective` to convert the original image to its bird's view, while `Minv` is used to do transform in the opposite direction. Here's an example of my output for this step of the test image:

![alt text][image13]
![alt text][image14]
![alt text][image15]





I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image24]
![alt text][image25]
![alt text][image26]



#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?


I use a histogram to find the locations of the maximum pixels along the `y-axis` and assume those are the locations of the points on the line. A `RejectOutlier` function is used to compare the `x-axis` value of all the points to their median value and reject the ones who are far away from the median. Last, a 2nd order polynomial is used to fit the lane lines such as:


Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image17]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.



The calculation of the radius of curvature of the line is obtained from the lecture. The vehicle position is determined by calculating the average `x coordinate` of the bottom left and bottom right points of the lines and comparing it with the middle point of the `x-axis`(i.e., 640). The deviation is then converted from pixels to meters. If the deviation is postive, vehicle is to the right of the center. If the deviation is negative, vehicle is to the left of the center. An example is shown as below:

![alt text][image16]

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

 Below are the final images for all six test images:

![alt text][image18]
![alt text][image19]
![alt text][image20]
![alt text][image21]
![alt text][image22]
![alt text][image23]



---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./result.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further. Below are the improvements I will try to make once I have more time:
   
 
 1. Get rid of the hard-coded numbers as much as possible and make the code robust.
 2. Tackle the challenge videos.
 3. Use neural network algorithms to detect these lines instead of just openCV.
