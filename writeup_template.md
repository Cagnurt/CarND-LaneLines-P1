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

### 1. Pipeline of the Project

My pipeline consisted of 5 steps:
1. Convert the images to grayscale
2. Make them blur with kernel size 5 to improve the performance of Canny Edge Detection
3. Apply Canny Edge Detection to detect lines
4. Because we are only interested in  only left and right lanes, not the rest of them, perform mask.
5. Use Hough Transformation to fit all detected right/left lines into a single line

draw_lines() function is significant in this pipeline. It uses a class: LineAnalysis.
Firstly, let's get into this class:
1.LineAnalysis takes lines, output of cv2.HoughLines(). 
  lines' shape is (#detected_lines, 4,  1) 
  4 is coming from up and bottom points' x and y values: x1, y1, x2, y2
2. Calculate m and b values for each possible lines (m*x+b=y)
3. Apply threshold for m to eliminate possible unidentified lines. Initialize "slopes" & "intercepts" attributes of the class with the filter passers
4. Right line's slope should be negative, left line's slope should be positive. According to m values, initialize "left_line_mask" & "right_line_mask".
5. Calculate mean of "m"s and "b"s for right and left lanes. 
Secondly, let's examine the draw_line():
1. Create LineAnalysis object. This class automatically calculate mean values. 
2. Because the lines start from bottom of the image, get this information, y_max
3. Mask operation in Pipeline-4 could limit for the upper part of the drawed lines, y_min. (Because we mask, cv2.HoughLines() could not return any lines smaller than this value.)
4. If any negative-slope line is detected in the object, find up and bottom points' x and y values: x1, y1, x2, y2 by using mean values. Draw it.5. If any positive-slope line is detected in the object, find up and bottom points' x and y values: x1, y1, x2, y2 by using mean values. Draw it.


### 2. Possible & Detected Shortcomings

1. The image processing methods, we applied in this project, are for spesific 
*light conditions
*camera calibrations
*camera angle
2. Changing lane case will be fail while changing.


### 3. Suggestions for Improving the Performance

1. Choose methods independent of the light conditions.
2. Change slope threshold (LineAnalysis-3)
3. Instead of mean (LineAnalysis-5), use different statiscal methods such as median, max, min. 

Another potential improvement could be to ...
