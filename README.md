# **Finding Lane Lines on the Road**
# Project 1 Submission by Samira Wettasinghe
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: masked_image.png "Masked Image"

[image2]: line_image.png "Line Image"

[image3]: final_image.png "Final Image"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The lane detection pipeline consisted of five steps. The first step was to convert the image to grayscale. Next a gaussian blur was applied to the image to suppress image noise with a kernel size of 5. The canny edge detection is now carried out with a low threshold of 50 and a high threshold of 150. This will produce many lines and therefore, it is required to focus on the lines that are in the general area where the lanes are located. We apply an image mask where all lines detected outside of a pre-defined quadrilateral will be filtered out. The mask will filter all data less than a height of 330 pixels. This was chosen because in general, the horizon is located just below 330 pixels.

![alt text][image1]

Once the mask has been applied to the image, a Hough transform is carried out to determine the endpoints of each line. Rho is chosen to be the highest resolution possible and theta is chosen to be 1 degree. The threshold was chosen to require 15 intersections in Hough space to be defined as a line. This was chosen since it seemed to yield the most robust results. The minimum line length and maximum line length was also chosen as 30 pixels and 20 pixels respectively.

![alt text][image2]

From the Hough transform, it is required to interpolate the lines as one left and right line representing the left and right lane. To do this, it is first required to filter out the lines that are near horizontal. To approximate the slope of each line segment, the SciPy linear regression algorithm is used. If this slope is less than 0.15, these end points are almost horizontal and thus ignored. If the minimum x-value (where x is the horizontal axis) is located to the right of the middle of the image, the line is categorized as a right lane. If the maximum x-value is located to the left of the middle of the image, the line is categorized as a left lane. Once categorized, all points in each respective lane is interpolated to find the best fit line using linear regression. This line is therefore, the left and right lane of the image. This image is merged with the original image to produce a final image with the lanes of interest highlighted in red.

![alt text][image3]

### 2. Identify potential shortcomings with your current pipeline

While this pipeline is good at detecting straight lines, it will have trouble detecting lines that are curved. Curved lines will occur while cornering. This is because the lane detection algorithm will attempt a linear equation to the cluster of endpoints from the Hough transform. In addition, the lane detection will ignore all horizontal lines. This will be an issue in intersections where it is required to know where the horizontal line is located to stop the vehicle.

In addition, this detection algorithm will be especially poor at detecting lanes in situations where the horizon moves (this will happen when vehicle experiences excessive body motion and downhill/uphill terrain).

Finally, this algorithm is far too slow. The video “solidWhiteRight.mp4” is an 8 second video however it took 61 seconds for MoviePy to render. This could be because MoviePy continuously writes to disk however, with a faster algorithm will result in a more robust self driving car.

### 3. Suggest possible improvements to your pipeline

To biggest fault in the pipeline is the linear approximation. Instead of using a linear function, a piecewise linear and square root function could be used. Where the square root function is used in the far field where the linear fit error becomes too large.

In addition, another improvement could be made in the mask. By using previous frames as a reference, the mask can be continuously modified frame by frame such that noise in the image such as dirt and construction markings can be easily ignored. This could be done by using the equation of lines approximated in the previous frame to focus in a smaller area of interest.
