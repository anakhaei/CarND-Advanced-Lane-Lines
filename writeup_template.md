## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./examples/binary_combo_example.jpg "Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"


### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

I used th eprovided chessboard images to calibrate the camera. as it was used in the coursed 

I start by preparing "object points" and "image points". Object points are 3D cordinates of the chessboard corners and and image points are 2D corners. By using 'cv2.findChessboardCorners', I can find the corners of the chess board and append them the image points vector. 

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.

This command resulted in the following matrix as the calibration matrix of the camera:

[[  1.15396093e+03   0.00000000e+00   6.69705357e+02]
 [  0.00000000e+00   1.14802496e+03   3.85656234e+02]
 [  0.00000000e+00   0.00000000e+00   1.00000000e+00]]

I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)
The pipleline of image processing includes:
* Distortion Correction
* Filter lane pixels
* Perspective transform
* Lane Boundry detection
* Determine the curvature of the lane and vehicle position
* Warp the detected lane boundaries back onto the original image.
* Disply lane curvature and vehicle position.


#### 1. Distortion Correction

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one. Here, I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result:

![alt text][image2]



#### 2. Filter lane pixels

I used a combination of color and gradient thresholds to generate a binary image. I implemented this step in block #41 in 'carnd_advanced_lane_line.ipynd' file. Here is an example of the applied filters in sample pictures:  

![alt text][image3]

#### 3. Perspective transform
This transormation will help us to see the top view of th eroad. To generate the matrix of transformation, I picked 4 points in one of the undistorded images and estimated their locations in a top view image. I used one image in which the lane is quite straight so it made life easier to estimate th eposition of the 4 points in the top view. 

Here is the image that I used. The 4 points are highlighted in the image:
![alt text][image3]

Here are the position of the src and dst points:

src = np.float32 ([[735, 470], [1145, 720], [260, 720], [583, 470]])
dst = np.float32 ([[1145, 0], [1145, 720], [260, 720], [260, 0]])

By using "cv2.getPerspectiveTransform" and "cv2.getPerspectiveTransform", I could calculate the matrix of transformation and also the inverse one. I implemented this step in block #41 in 'carnd_advanced_lane_line.ipynd' file. 

I used the transfromation matrix to transform the following images:



![alt text][image4]

#### 4. Lane Boundry detection

I used almost the same code provided in the course to fit my lane lines with a 2nd order polynomial kinda like this. 

![alt text][image5]

#### 5. Determine the curvature of the lane and vehicle position

By using the estimation of 

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
