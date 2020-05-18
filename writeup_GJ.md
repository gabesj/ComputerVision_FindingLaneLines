# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/highlighted_solidWhiteCurve.jpg "Output"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

To process the images, my code performs the following:

A. Makes a copy of the image which only includes the yellow and white pixels
   * Creates a copy of the image in HSV color space
   * Creates a copy of the image capturing only yellow pixels
   * Creates a copy of the image capturing only white pixels
   * Combines the two new images into a single image containing only yellow and white pixels
     
B. Performs a Gaussian blur operation on the image of yellow and white pixels
 
C. Performs a Canny edge detection on the blurred image
 
D. Defines a region of interest and creates an image of the Canny edges within that area
 
E. Performs a Hough transformation to identify line segments on the Canny image
 
F. Creates an empty image and draws lines to represent the original image's lane lines.
 
   1. If the code asks for line segments:
      - Creates a blank image
      - Draws the line segments determined by the Hough transform
   2. If the code asks for solid lines:
      - Calculates the slopes of each line determined by the Hough transorm
      - Performs a rough sort based on slope, grouping into candidates for left and right lane lines
      - Finds the median slope in each of those two groups and assumes that is the slope of the lane line
      - In both groups: Finds the line that matches most closely with the slope and determines its midpoint.
      - In both groups: Determines a line across the entire image based on that midpoint and the slope
      - Draws those two lines on a blank image
      - Defines the region of interest and creates an image of the lines in that area
      
G. Overlays the image containing the lines onto the original image

![alt text][image1]
 
 
### 2. Some limitations of this project approach and my code include the following:

 - Lane lines may not be visible when cresting a hill
 - During a lane change, the region of interest is no longer ideal.
 - I make rough grouping judgements based on slope that would not properly identify a line during a lane change
 - There are frames of the video where a lane line is not found
 - Driving closely behind another car may make lane lines less visible.


### 3. Some suggestions that would improve my code include:

 - The lines of best fit sometimes jump around from one side of the lane marking to the other parallel side from frame to frame in videos.  I could add a feature to prioritize the matching of new lines in similar positions to the previous frame's lane line.  Or I could independently find a new line in the current frame, and as long as the slope is similar to that of the previous frame, I could choose a best fit line straddled by the two.
 - I could create two horizontally stacked regions of interest instead of one, allowing me to find two left lane lines and two right lane lines.  This coud be useful for sharp curves.  I could have one region of interest representing 0-50 feet in front of the car and another region of interest representing 50+ feet in front of the car.  This would give me an angular approximate to the upcoming curve, and while still lacking in accuracy, could be more useful than a single set of lines.

