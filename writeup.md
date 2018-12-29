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

[image1]: ./writeup/compare.png "Undistorted"
[image2]: ./writeup/cali_real.png "Road Transformed"
[image3]: ./writeup/color.png "Binary Example"
[image4]: ./writeup/wrap.png "Warp Example"
[image5]: ./writeup/result.png "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./result/project_result.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README


### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./AdvancedLaneLines.ipynb" .  

This problem started out by trying to improve the Lane line detection from the first project.  So the first technique that needed to be employed was that of calibrating the camera.  This was done by using the chessboard images included with this project.  The images were first converted into grayscale and then used to a function of open cv to find the corners of the squares on the chessboard.  See images in the below. 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one(this step at cell 13):
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at cell 16 in `./AdvancedLaneLines.ipynb`).  Here's an example of my output for this step. 

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `perspective_transform()`, which appears at cell 14 in `./AdvancedLaneLines.ipynb` :

```python
src = np.float32([[492, 485], [805, 485],
                  [1245, 720], [42, 720]])
dst = np.float32([[0, 0], [1280, 0],
                  [1250, 720], [40, 720]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 492, 485      | 0, 0          | 
| 805, 485      | 1280, 0       |
| 1245, 720     | 1250, 720     |
| 42, 720       | 40, 720       |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Where the color of the road has been remove since this is a grayscale image and checked for color intensity.  The next step was to fit a polygon to where the predicted lane lines were this will help to determine the calculations, by knowing the size of the polygon the program can then calculate where it is in relation to the image and with that information it can then calculate the curvature of the road.   In order to get an accurate value of the curvature of the road it is displayed on the screen every four frames, also a sliding search is used to make sure the lane lines are accurately being determined.this step at cell 18 in `./AdvancedLaneLines.ipynb`.


#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this also in at cell 20 in `./AdvancedLaneLines.ipynb`.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step at cell 19 in `./AdvancedLaneLines.ipynb`.  Here is an example of my result on a test image:

![alt text][image5]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./result/project_result.mp4)
but in challange video its perform not really well,that might because I'm overfitting :D

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why,my future work may include below:

* experiment with LAB color space to determine whether we can produce better color thresholding
* try to tune my parameters make my code more robust
* try other computer vision methods
