# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.
 

The project objective is to detect lane lines in images using Python and OpenCV and finally applying it to a video.
OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images



The Project
---

### Reflection

### 1. Description of my pipeline.



My pipeline consisted of 5 steps. 

1. I converted the images to grayscale, using the helper function grayscale(img)

2. Gaussian smoothing is done with kernel size = 5 on the grayscaled image obtained from previous step. This step is done for      getting a better result in step 3.
 
3. Applied Canny Edge Detection Algorithm to find the edges of objects to optimize the detection of the lane lines well without    detecting a lot of other stuff. The recommended ratio of the minimum and maximum threshold is 1:2 or 1:3. I am getting better    result in minimum threshold 50 and maximum threshold as 150.
    
4. Created a masked edges image identifying the area of our interest in the image to find the lane. I used 4-sided polygon to      mask the region using the function cv2.fillPoly().

5. Finally, I applied Hough Space Transformation to detect the lines on the masked area of the image. The value of rho, threshold, minimum line length and the maximum line gap taken are 2, 15, 20 and 10 respectively, which turned out to work fine for me. The theta value was usual (pi/180).



In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first calculating the average of all the slope and intercepts of all the lines detected using hough transform and then finding the position of the bottom "x" and the top "x" for both the left and right lane.





### 2. Potential shortcomings with the pipeline


One potential shortcoming would be what would happen when the camera will have a different angle and position for the road. It would be diffcult to find the lane lines as it would not match the region we masked in the pipeline. 

Another shortcoming is that it will work well on shadowed areas. We would need different value of kernel sizes for that while smoothing using gaussian smoothing.






### 3. Possible improvements to the pipeline

A possible improvement would be to to find out all the possible position of the lane on the road with different obstacles.
Maybe using neural networks, it would help much better