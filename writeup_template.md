
# Advanced Lane Finding Project

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

[image1]: ./output_images/figure_1_undistorted.png "Undistorted"
[image2]: ./output_images/figure_2_undistorted_road.png "Road Transformed"
[image3]: ./output_images/figure_3_warped_image.png "Binary Example"
[image4]: ./output_images/figure_4_warped_binarized.png "Warp Example"
[image5]: ./output_images/figure_5_warped_back.png "Fit Visual"
[video1]: ./project_output.mp4 "Video"

A jupyter notebook was used. Documentation of different functionalities of the code is well commented there and is not repeated here. 
  
### Camera Calibration

I used provided chessboard images for calibration. Opencv provided a function to detect chessboard corners and these were compared to a known fact that corners should have regular positions to one another. 20 images can then be used to get the calibration and distortion coefficients. Example undistorted image is provided.  
![Undistorted chessboard image][image1]

### Pipeline (single images)

I then applied the undistortion to road test images, an example is shown here.
![Undistorted road image][image2]

#### Perspective transform

The next step was perspective transform. I identified from the test images proper source points that should represent a rectangle. I then chose destination points (corners of the image) where the rectangle points should be mapped. Image warping was then used to move from front view to bird's eye view. Contrary to the Udacity pipeline, I did warping first followed by color and gradient thresholding. 


I verified that my perspective transform was working as expected by verifying that the lines appear parallel in the warped image.
![alt text][image3]

#### Color and gradient thresholding

I used a combination of color and gradient thresholds to generate a binary image. Sobel x-gradient and a HSL color space transform were used. In color thresholding only the saturation channel was considered with values s_thresh=(170, 255). The Gradient threshold was sx_thresh=(20, 100). 
 	



####  Line pixel identification

Lane-line pixels where identified from the binary image by computing a histogram of pixel x-values. By knowing that there are two lane lines, one can assume a bi-modal distribution and its two peaks can be considered to be the two lanes. 

Degree-2 polynomials were then fitted to each collection of pixels corresponding to the two lane lines. In figure 4 I show a binary image where the polynomials have been fitted after warping and binarization. 
![Binary transformed image with lanes detected and fitted][image4]

#### The radius of curvature
The radius of curvature of the lane lines was calculated from the polynomial fit parameters using the formula provided in the course material. The transformation from pixels to real-world metric values was included. The position of the vehicle with respect to center of the road was calculated by taking average of the intercepts of the two lanes and calculating its distance from the center of the image, knowing the camera should be in the center of the car. A transformation to metrix values was included. 

An example image with the lane plotted and warped back to the front view is provided in image 5. 

![Example image warped back to front view after the lanes were detected in bird's eye view][image5]

---

### Pipeline (video)

1. A link to my final video output with lanes identified is provided here [link to my video result](./project_output.mp4)

---

### Discussion

The detection algorithm works reasonably well in standard conditions. Sometimes there are frames where the lane cannot be detected and sometimes there are difficult disturbances in the road, such as the lane-resembling asphalt border popping out in the challenge video. I worked a bit on smoothing and discarding bad frames but did not have time to build a perfect solution. 

