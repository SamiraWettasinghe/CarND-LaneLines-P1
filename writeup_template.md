# **Finding Lane Lines on the Road**
# **Project 1 Submission by Samira Wettasinghe
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

The lane detection pipeline consisted of five steps. The first step was to convert the image to grayscale. Next a gaussian blur was applied to the image to supress image noise with a kernel size of 5. The canny edge detection is now carried out with a low threshold of 50 and a high threshold of 150. This will produce many lines and therefore, it is required to focus on the lines that are in the general area where the lanes are located. We apply an image mask where all lines detected outside of a pre-defined quadrilateral will be filtered out. The mask will filter all data less than a height of 330 pixels. This was chosen because in general, the horizon is located just below 330 pixels.

Once the mask has been applied to the image, a Hough transform is carried out to determine the endpoints of each line. Rho is chosen to be the highest resolution possible and theta is chosen to be 1 degree. The threshold was chosen to require 15 intersections in Hough space to be defined as a line. This was chosen since it seemed to yield the most robust results. The minimum line length and maximum line length was also chosen as 30 pixels and 20 pixels respectively.

From the Hough transform, it is required to interpolate the lines as one left and right line representing the left and right lane. To do this, it is first required to filter out the lines that are near horizontal. To approximate the slope of each line segment, the SciPy linear regression algorithm is used. If this slope is less than 0.15, these end points are almost horizontal and thus ignored. If the minimum x-value (where x is the horizontal axis) is located to the right of the middle of the image, the line is categorized as a right lane. If the maximum x-value is located to the left of the middle of the image, the line is categorized as a left lane. Once categorized, all points in each respective lane is interpolated to find the best fit line using linear regression. This line is therefore, the left and right lane of the image. This image is merged with the original image to produce a final image with the lanes of interest highlighted in red.

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ...

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
