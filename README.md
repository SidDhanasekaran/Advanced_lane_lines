## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


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

[image1]: ./output_images/undistorted_image.jpg "Camera Calibration"
[image2]: ./output_images/pipeline_output.jpg "Threshold Binary Image"
[image3]: ./output_images/warped_image.jpg "Warped Image"
[image4]: ./output_images/lane_curvature.jpg "Lane curvature Annotation"
[video1]: ./project_video_output.mp4 "Project Video"


###Camera Calibration

####Step 1. Compute the camera calibration using chessboard images. 
#####p4_advanced_lane_lines.ipynb Step 1

We will be using the the following OpenCV fuctions to calibrate the camera images provided:
cv2.findChessboardCorners()
cv2.drawChessboardCorners()
cv2.calibrateCamera()
For additional details on the above functions please read this tutorial:

http://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_calib3d/py_calibration/py_calibration.html

####Step2. Apply a distortion correction to raw chessboard images.
#####p4_advanced_lane_lines.ipynb Step 2

The cv2.calibrateCamera() fuction provides the `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

####Step 3 : Pipeline (Use color transforms, gradients, etc., to create a thresholded binary image.)

We start with defining the region of interest similar to project 1 and defining the pipeline for edge detection.

Pipeline to create a Threshold binary image: 

- Gaussian Blurring to reduce noises
- Sober Operator x-direction
- Sober Operator y-direction
- Direction of Gradient
- Magnitude of Gradient
- HLS Color Threshold


The Sobel Operator is a discrete differentiation operator. It computes an approximation of the gradient of an image intensity function. The Sobel Operator combines Gaussian smoothing and differentiation.

![alt text][image2]

####Step 4 . Apply a perspective transform to rectify binary image ("birds-eye view")

The code for warping the image is implemented in the imagewraping() function. The  function takes an input image as well as source (`src`) and destination (`dst`) points to do a perspective transform.  I chose the hardcode the source and destination points.

![alt text][image3]

####4. Detect lane pixels and fit to find the lane boundary and fitted with a polynomial.
From the warped image I could divide the lane line points into left and right lane line points. Then I fitted 2nd order polynomial to each lane line using `numpy.polyfit()` function. 

####5. Determine the curvature of the lane and vehicle position with respect to center and warp the detected lane boundaries back onto the original image.

Using the fitted 2nd order polynomial parameters, the lane can be drawn and warp back to the original undistorted image. At the same time vehicle position and curvature of the lane can be found.
![alt text][image4]

---

###Applying the Image Processing pipeline on the Video
 I have used the moviepy.editor to apply the Image processing pipeline to the recorded video provided by Udacity. 

![Final Result Gif] (./project_video_output.mp4)

---
##Discussion

I learned a lot about the various thresholding techniques to make the pipeline work. Below are some of the challenges I thought were interesting: 

- The ability to accurately identify pixels that belong to the lane lines. The process relies on having a good binary threshold that can separate the lane lines from the rest of the background image. 
- Getting the pipeline to be robust against shadows and at the same time capable of detecting yellow lane lines on white ground. 
- The images and the video had good lighting, I would be interested in playing with images in different weather conditions and bad lighting. 
- I have hard coded the region of interest and source and destination points. I would like to look at how I can dynamically generate those values. 


Overall this project was a good break from Deep learning and was a lot of new things I have learned :)
