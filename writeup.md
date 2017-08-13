## Advanced Lane Finding

### This is forth project of Self-Driving Car Engineer Nanodegree Program.

---

**Advanced Lane Finding Project**

The steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Defined color transforms and gradients function, and find a combined method function to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Finally, Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/undistorted_calibration1.jpg "Chess Image"
[image2]: ./test_images/test2.jpg "Test image"
[image3]: ./output_images/source_points.jpg "source"
[image4]: ./output_images/destination_points.jpg "destination"
[image5]: ./output_images/warped_test2 "Warp result"
[image6]: ./output_images/good_line_find.jpg "Good Line Find"
[image7]: ./output_images/bad_line_find.jpg "Bad Line Find"
[image8]: ./output_images/final_result.jpg "Final Result"
[video1]: ./project_video.mp4 "Video"


### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

As this lesson shown，use openCV to calibrate camera and undistorate images. The process start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection. Then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and sobel x gradient to generate a binary image. I tested in the test images. 
![alt text][image5]

#### 3. Describe how you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warp_image()`, which appears in in eithth code cell of the IPython notebook located in "./SolutionImplement.ipynb".  The `warp_image()` function takes as inputs an image (`img`), as well as source (`src_point`) and destination (`dst_point`) points.  I chose the hardcode the source and destination points in the following manner:


This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 596, 450      | 310, 120        | 
| 210, 740      | 310, 740      |
| 1200, 740     | 1100, 740      |
| 685, 450      | 1000, 120        |



I verified that my perspective transform was working as expected by drawing the `src_point` and `dst_point` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image3]
![alt text][image4]

#### 4. Describe how you identified lane-line pixels and fit their positions with a polynomial?

I did some other stuff and fit my lane lines with a 2nd order polynomial kinda

I defined the `find_lane_lines` function in "./SolutionImplement.ipynb".

#### 5. Describe how you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in function `get_curvature` of the IPython notebook located in "./SolutionImplement.ipynb".

But I found the effect of this was not good. Like below:

![alt text][image7]

So I change the combined function which in eleventh code cell of the IPython notebook located in "./SolutionImplement.ipynb". And I combined the HLS L-channel and LAB B-channel color transform together. The effect of this combined transform is more better than above. Like below:

![alt text][image6]

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in seventeenth code cell of the IPython notebook located in "./SolutionImplement.ipynb".  Here is an example of my result on a test image:

![alt text][image8]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?


* When I apply in challenge video, the output is not good. I think the reason is in combination function is not well enouph. For more better output, we should find a better combination to improve the effect of detect line. Also, we should do some Preprocessing like Histogram equalization.

