# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

I started by defining a rough colour range that captures yellow and white while discarding other colour information. Next, I converted the images to grayscale. I then applied the Gaussian blur, detected edges using the canny function,
reduced the image to an area of interest and finally applied the Hough transform to detect lines. The initial parameters were taken from the Udacity lessons and were tweaked a little to improve detection of smaller
line segments.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function and added some helper functions. The left-hand and right-hand lines were initially identified using their slope 
(as suggested in the comments). Next, for each side all points related to the identified line segments were added to lists and used as input for fitting a linear function.
This function was used to draw a line between the upper and lower region of interest boundaries. The result was applied to the raw image using the weighted_add function.
This did not work well when the Hough transform found smaller line segments with random slopes that were attributed to the wrong side. Eventually I chose the very simple method of using distance from the image middle 
to determine the side the line belongs to. Attempts to make the pipeline work on the more challenging video led to more parameter fine-tuning, still does not work perfectly though.


![example](/test_images_output/t2.jpg)



### 2. Identify potential shortcomings with your current pipeline

The pipeline depends on guesstimated parameters. The detection of line segments as belonging on the left or right is currently very simple.

The pipeline is not robust to changing conditions such as environmental conditions:
driving at night, through fog or over snow may mess up the edge detection which relies on contrast. Reflections or obscuring objects (snow, traffic jam) may confuse this pipeline.
The region of interest currently does not work equally well for likely scenarios (curves, up-hill, down-hill). 
Also, the road marking may not be consistent depending on type of road or country.
If the car switches from one lane to another or makes a turn the results will probably be incorrect temporarily.
No exception-handling or logging.
 
### 3. Suggest possible improvements to your pipeline

Switching to another colour space might help with changing light conditions. 
Using additional information (position of other vehicles) may also indicate the lane when the lane lines are not visible.
Defining the region of interest as function of some sensor may make the pipeline more robust.
Detecting other types of borders for lanes (board walk, posts) could be useful in other environments.
Detecting the road instead of the lane lines could provide additional safety (if no lanes found try to stay on right-hand patch of road).
Add exception-handling and logging.
Fit higher-order polynomial instead of lines to help with curves.
