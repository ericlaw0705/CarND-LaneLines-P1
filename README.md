# **Finding Lane Lines on the Road** 

## Writeup For P1

### By Eric Law

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Create a pipeline that finds lane lines on the road, represented by colored images or a video (series of images)
* Reflect on my work in a written report
* Identify the potential shortcomings of my current pipeline to find such lane lines
* Suggest possible improvements to my pipeline


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Descption of my pipeline

First and foremost, the environment needed to be setup by importing the libraries that contain the necessary methods to perform most of the required algorithms, most notably the cv2 (openCV), numpy, and the standard math library. Then, my pipleline consisted of the 6 main steps as described in the *Computer Vision Fundamentals* module:

1. A source image is converted to grayscale
2. A Gaussian blur is applied with a selected kernel size
3. The Canny algorithm is then applied on top to convert the image into a gradient and find the edges
4. A region of interest is selected from the resulting image, with areas outside being masked out
5. Apply Hough transform to the masked and edge-detected image
6. The edges are then superimposed onto the original image

### 2. How *draw_lines()* was modified

Initialliy, the *draw_lines()* method was simply to draw all short line segments found by the Hough Transform. In order to display a continuous line that resemble the actual lane line found, I first filitered out potential false positives by only looking into lines within a certain range of slope values. For example, and edge found to have a slope close to 0.0 would likely not be a lane line as lane lines run longitudinally along the travesal direction of the vehicle. From the camera's perspective, and as evident in the sample images, the lane lines tend to have a particular slope value towards a point of convergence. 

Next, the slope values within the range are continually averaged (both for left and right lane lines separately), along with the mid points of the line segments.

Using the averaged slope and points, the entire line that represent the lane line is created by extrapolating up to the edge of the region of interest.

### 3. Observations, potential shortcomings, and possible improvements

* Short-dashed lane lines and solid lane lines require different parameters to be detected, goal is to find the sweet spot, could be a variable instead of a hardcoded value.
* Region of interest varies with the type of images, and is highly dependent on the orientation of the vehicle (turning or not, going up or down a hill, etc). Knowing the pitch, row, yaw, and even an estimation of the grade of the road would greatly help lane line detection
* Region of interest also depends on the size of the image (as I found out in the challenge video that the dimension of the video is different than the previous ones), and where the line of convergence is. If cameras are mounted slightly left or right of center, this would "break" my current pipeline. The exactly physical dimension/configuration of the camera would help determine the region of interest as well.
* Taking the average of the edges found is probably not the best representation of all the lines that are found, but does a decent job in filtering out some false positives due to our non-robust selection of parameters. Line lanes, more often than not, are curved and not completely straight. Using a straight line segment to represent lane lines do not work all the time.
