
**Advanced Lane Finding Project**

I put all the code into a iPython Notebook for easy marking (rather than encapsulate the code in multiple classes if I was writing production code)

Each Step has a corresponding section in the iPython Notebook Sections 1-8

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images. (Section 2)
* Apply a distortion correction to raw images.(Section 2)
* Use color transforms, gradients, etc., to create a thresholded binary image. (Section 3 & 4)
* Apply a perspective transform to rectify binary image ("birds-eye view"). (Section 5)
* Detect lane pixels and fit to find the lane boundary. (Section 6 Parts 1-5)
* Determine the curvature of the lane and vehicle position with respect to center. (Section 6 Part 4)
* Warp the detected lane boundaries back onto the original image. (Section 6 Part 5)
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position. (Section 7)
* Created the Video (Section 8)

[//]: # (Image References)

[image1]: ./writeup_images/distorted_chessboard.png
[image2]: ./writeup_images/undistorted_chessboard.png
[image3]: ./writeup_images/distorted_image.png
[image4]: ./writeup_images/undistorted_image.png
[image5]: ./writeup_images/masked_image.png  
[image6]: ./writeup_images/thresholded_image.png  
[image7]: ./writeup_images/perspective_transformed.png  
[image8]: ./writeup_images/perspective_transformed_binary.png
[image9]: ./writeup_images/lane_overlay.png    

[video1]: ./project_video.mp4 "Video"
[video2]: ./project_video_submission.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.   
You're reading it!
Please refer to ipynb file for the code in each section

###Section 1 Imports

This is to import all the required python libraries.

###Section 2 Camera Calibration and Distortion correction

Using the files in given in the camera_cal directory, I used the cv2.findChessboardCorners to find 9,6 corners respectively.

Next I used the cv2.calibrateCamera to calibrate the camera and defined a undistort function that uses
cv2.undistort to undistorted a given image.

Results are as follows:

Distorted Chessboard   ![alt text][image1]

Undistorted Chessboard ![alt text][image2]

Distorted image        ![alt text][image3]

Undistorted image      ![alt text][image4]


###Section 3 Image mask

Taking the undistorted image, I applied a mask by defining a region of interest (dimensions in the code). The rest of the image was changed to black. The results are as follows:

Masked Image      ![alt text][image5]

This had the effect of removing some noise on the sides of the image as lane detection is very sensitive to noise close the lanes

###Section 4 Thresholding

I first tried the techniques detailed in the lectures such as the sobel, finding the magnitude and finding the gradient. I tuned these with various parameter values but ultimately did not get the best result as there were just too many parameters to tune. In many cases ,the overlay exceeded the lane lines or was out of shape. I believe that this was due to noise during the thresholding process.

I then tried a different approach by reducing the number of parameters to tune.

a) I already have a masked image from the earlier step

Masked Image      ![alt text][image5]

b) I converted the image to HLS colorspace which would allow better detection of hues specifically the yellow and white that make up the lane lines

c) I applied a yellow color and white color mask using cv2.inRange method and combined the two

d) The resulting thresholded binary image is as follows:

Thresholded Image      ![alt text][image6]


###Section 5 Perspective Transform

I carried out the perspective transform using the method detailed in the lectures as got these results

undistorted Image     ![alt text][image4]

Perspective transformed     ![alt text][image7]

Perspective transformed Binary      ![alt text][image8]

###Section 6 Lane Detection, Curvature, Offset

I applied the histogram calculation method and determined the two highest peaks as leftx_base and rightx_base to find the initial lane boundaries to work with. The rest of the method I learnt from the lectures.

For the curvature, I just averaged out the left and right curve radius.
  curvature = (left_curverad + right_curverad)/2

And for the offset, I just calculated the image midpoint (1280/2=640) and used the formula:   
    offset =((rightx_base-640 )-(640-leftx_base))*xm_per_pix
    where xm_per_pix = 3.7/700 (as recommended in the lectures)

I got the results that is shown in the next section

###Section 7 Visualising Lane Detection Overlay

Final Result     ![alt text][image9]


###Section 8 Creating the video

I had some problems creating the video due to my old computer(keeps hanging when running the video processing). I just output the video separate frames (project_video_submission_temp directory) and process each overlay into (project_video_submission directory) using the write_images_sequence.

Finally I combined the images into a video using the same technique and video.py file from project 3. (Software Resue!)

Results are in the video "project_video_submission.mp4"

###Discussion

Schedule was tight, did not have time to work on the challenge videos. There are too many parameters to consider. Would have liked more time to read through all the cv2 documentation, thresholding techniques and parameters.   

Thank you.
