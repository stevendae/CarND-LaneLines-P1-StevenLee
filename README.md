# **Finding Lane Lines on the Road** 
---

## Project Description

In this project, I used computer vision techniques to identify the trajectory of dashed lane lines on images/videos of traffic roads. 
The goals / steps of this project are the following:

* Make a pipeline that finds lane lines on the road
* Write reflections in a written report

**Method**

The following libraries (included in environment.yml) were used to achieve the goals of this project.
- Matplotlib.pyplot (MATLAB-like plotting framework for visualization)
- Matplotlib.image (Image loading, scaling, and display options)
- NumPy (processing data in the form of arrays)
- OpenCV (computer vision applications)

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale so that when the Canny Edge Detection algorithm is applied the algorithm operates on a single color channel, then I applied a Gaussian Blur with a kernel size of 5 to reduce the noise level to further isolate and highlight the gradients detected in the Canny Edge algorithm. Then I applied a Canny Edge algorithm with a low threshold of 50 and a high threshold of 150 as somewhat standardized or recommeded by John Canny himself. Then I created a region of interest where I defined points that, when connected, made a trapezoid like figure to create a boundary within the frame to further precisely detect our objects of interest that being the converging lane lines. Then I set my Hough transform parameters where I had a distance resolution of 1 in the pixels of the Hough grid, an angular resolution of a single radian, a threshold of 18, and a minimum number of pixels making up a line and max gap between pixels to connect a line segment to 5 and 10 respectively. I ran the hough transform function, where nested in the function itself was the draw_lines function. I modified the draw_lines function such that rather than drawing a line for every line denoted in the 'lines' array I would separate each 'line' to fall under the left or right line set. If the slope of the 'line' was negative it belonged to the left liine, if the slope was positive it belonged to the right line. Once I had my set of points for both left and right categories I used the polyfit and poly1d functions from the numpy module. The polyfit function uses a regression technique to identify a line of best fit and outputs the parameters of slope and intercept. The poly1d function takes the outputs of the polyfit function as inputs and then creates a math function to identify the y point of a given x on that line of best fit. The polyfit function was used to average all the lines together, and the poly1d function was used to mark the appropriate points for my single averaged line to be drawn. Considering that the line should stretch right to the vertical bounds of the region of interest I assigned a y-value of 540 for the base points and the inverse equation of x as, y-(y-intercept)/slope, to find x. Lastly I projected these lines onto the imported image and produced a function to export my result. 


### 2. Identify potential shortcomings with your current pipeline

Potential shortcomings with the pipeline is that since the region of interest is fixed it restricts the scope of attention to only the data in the region. A fixed region of interest is not the most ideal considering that the vehicle will be exposed to different driving environments. Another shortcoming is that there is no mechanism that has been introduced to deal with outliers of data, therefore there is no embedded consolation to assess outlier input data.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to assign regions of interest based on dynamic input data that detects the environment in which the vehicle is operating in. 
Another potential improvement could be to implement a decision making algorithm to assess outliers and adjust accordingly, rather than adapting to continous new data.