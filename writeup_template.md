# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Description of the pipeline.

Here are the stages of my lane detection pipeline... 

1. Convert image to grayscale.

*  Run Canny edge detection on the grayscale image.

*  Create a region of interest mask to exclude any unwanted lines (false 
   positives). Use this mask to crop the image to a sub-section of the image
   where the lanes are most likely to appear.

*  Use Hough transform on the masked image to detect lines.

*  Use draw_lines() to further whittle down multiple lines into lanes.


In order to draw a single line on the left and right lanes... 

I created a function draw_average_line() to take slopes and y-intercepts as input and ...

1. Calculate the average slope and y-intercept

*  Draw a line with the calculated average slope and y-intercept

Then I modified the draw_lines() function to ...

1. Calculate the slopes of all lines detected by the Hough transform. 

* Reject any lines which fall outside the slope interval (0.5, 1.5).

* Collect all lines with negative slope as possible candidates for the left lane and all lines with positive slope as possible candidates for the right lane.

* Draw the left and right lanes using draw_average_line().


### 2. Shortcomings of the current pipeline

1. Converting the image to grayscale looses color information. The lines may not always be yellow or white.  

* The region of interest mask uses an arbitrary polygon chosen to work with the test videos supplied with the assignment. The mask works well only with fairly straight stretches of the road.

* The lanes bend and curve with the curvature of the road hence they cannot always be represented with straight lines. A segmented line (piecewise linear) or a bezier curve may be a better approximation.

* Lanes on unmaintained roads wear off with time. Also parts of a stippled lane line may be missed by the camera on a fast moving car (especially if the stippling frequency is a multiple of the camera frame rate). 


### 3. Improvements
1. We could probably do a better job at lane detection if we took the color of the lanes into account. Convert to HSL or HSV and make use of hue to select possible lane colors.

* Get rid of the mask and detect all lanes and choose the most probable lines from them.

* A segmented line (piecewise linear) or a bezier curve may be a better approximation for lane lines.

* Make use of information from previous frames to fill in missing sections of the lanes.
