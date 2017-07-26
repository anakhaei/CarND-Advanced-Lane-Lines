## Advanced Lane Finding Project**

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

[image1]: ./images/undist_chess.png "Distortion correction"
[image2]: ./images/undist_images.png "Distortion correction"
[image3]: ./images/bin_img_1.png "Binary Example"
[image4]: ./images/perspective_mat.png "Perspective Transform Matrix Calculation"
[image5]: ./images/perspective_2.png "Warp Example"
[image6]: ./images/windows.png "Fit Visual"
[image7]: ./images/processed_img.png "Draw Lane"
[video1]: ./project_video.mp4 "Video"


### Camera Calibration

I used the provided chessboard images to calibrate the camera. I start by preparing "object points" and "image points". Object points are 3D cordinates of the chessboard corners and and image points are 2D corners. By using 'cv2.findChessboardCorners', I can find the corners of the chess board and append them the image points vector. 

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.

Here is the result of distortion correction on one of the chess board images by using the `cv2.undistort()`: 

![alt text][image1]

### Pipeline (single images)
The pipleline of image processing includes:
* Distortion Correction
* Filter lane pixels
* Perspective transform
* Lane boundry detection
* Determine radius of curvature
* Warp the detected lane boundaries back onto the original image.

#### 1. Distortion Correction

To demonstrate this step, I apply my distortion correction function to the test images. I implemenetd the code of distortion correction in 'Undistort Images' section of 'carnd_advanced_lane_line.ipynb' file. My distoration correction function uses `cv2.undistort()` and the camera calibration matrix to undistort the images:

![alt text][image2]


#### 2. Filter lane pixels

I used a combination of color (saturation) and gradient thresholds to generate a binary image. I implemenetd the code in 'Filter Images' section of 'carnd_advanced_lane_line.ipynb' file. Here is an example of the applied filters in sample pictures:  

![alt text][image3]

#### 3. Perspective transform
This transormation will help us to see the top view of th eroad. To generate the matrix of transformation, I picked 4 points in one of the undistorded images and estimated their locations in a top view image. I used one image in which the lane is quite straight so it made life easier to estimate th eposition of the 4 points in the top view. 

Here is the image that I used. The 4 points are highlighted in the image:

![alt text][image4]

Here are the position of the src and dst points:

src = np.float32 ([[735, 470], [1145, 720], [260, 720], [583, 470]])

dst = np.float32 ([[1145, 0], [1145, 720], [260, 720], [260, 0]])

By using "cv2.getPerspectiveTransform" and "cv2.getPerspectiveTransform", I could calculate the matrix of transformation and also the inverse one. I implemented this step in block #41 in 'carnd_advanced_lane_line.ipynd' file. 

I implemenetd the code in 'Perspective Transform' section of 'carnd_advanced_lane_line.ipynb' file. I used my perspective transform function to transform the following images:

![alt text][image5]

#### 4. Lane Boundry detection

I used almost the same code provided in the course to fit my lane lines with a 2nd order polynomial kinda like this. I implemenetd the code in 'Curveture Estimation' section of 'carnd_advanced_lane_line.ipynb' file.

![alt text][image6]

#### 5. Determine the radius of curvature

I used the same code and equations provided in the course to calculate the curviture. I implemenetd the code in 'Curveture Estimation' section of 'carnd_advanced_lane_line.ipynb' file.

#### 6. Warp the detected lane boundaries back onto the original image.

I implemenetd the code in 'Curveture Estimation' section of 'carnd_advanced_lane_line.ipynb' file in the The 'draw_lane()' function. 

![alt text][image7]


### Pipeline (video)

I have applied my pipeline on the project video in the folder. Here's a [link to my video result](./project_video.mp4)

### Discussion
This approach worked quite well on the simple video provided in the course. However, I have tried it as is on the harder challenge video and it could not detect the lane properly. Issues like presence of bikes and more car, light conditions and reflection of sun light, smaller radius of curvature contribute to the complexity of the video. 
  
