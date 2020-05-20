# **Finding lane lines on the road - a simple lane detection pipeline** 

[//]: # (Image References)

[image1]: ./plot_saves/1_image.jpg "Input image"
[image2]: ./plot_saves/2_grayscale.jpg "Grayscale image"
[image3]: ./plot_saves/3_gray_select.jpg "Grayscale image filtered by intensity"
[image4]: ./plot_saves/4_gray_select_roi.jpg "Filtered image reduced to the relevant ROI"
[image5]: ./plot_saves/5_img_canny.jpg "Image after canny-edge detection"
[image6]: ./plot_saves/6_canny_blur.jpg "Image after canny-edge detection and blur to strengthen edges"
[image7.0]: ./plot_saves/7.0_hough.jpg "Unused output image of the hough line transform - to show detected line-segments"
[image7.1]: ./plot_saves/7.1_lane_img.jpg "Image of extrapolated lines that represent the detected lanes"
[image8]: ./plot_saves/8_image_annotated.jpg "Original image with annotated lanes"
---


### 1. Pipeline processing step by step

My pipeline consists of 8 steps:

First, I loaded the input image. 
![alt text][image1]

As next step I converted the images to grayscale. 
![alt text][image2]

Third, I filtered the image by intensity with the `inRange()` function to filter out pixels that are to dark for being lane markings. 
![alt text][image3]

The fourth step was setting a ROI (region of interest) along the area of the image where the lane markings of our lane can be. 
![alt text][image4]

As the fifth step, I used opencv's `canny()` function to detect the edges of the lane markings. 
![alt text][image5]

To improve the output of the edge detection I applied a gaussian blur of kernsel size 3 to strengthen the edges.
![alt text][image6]

After the edge detection I used the hough transform for finding lines. 
This image is displayed just to show the resulting lines defining the lane markings generated with my parameters. I did not use this output image.
![alt text][image7.0]

For this sixth step I instead adapted the `hough_lines()` function and added the lines as a return value. 

In order to draw a single line on the left and right lanes, I used the `draw_lines()` function within my own `extrapolate_lanes_image()` function as the seventh step. This function orchestrates the call of some other functions that first seperate the left from the right lines. Next I call my `extrapolate_lines()` function seperately on the left and then the right lines. For extrapolating the lines I calculate the average slope, the average y-Axis intersection and with that the average intersection with my set ROI boundarys. With these average values and the roi intersections for the left and the right lane I use the `draw_lines()` function to draw one single extrapolated line for each lane.
![alt text][image7.1]

Last but not least in step eigth, I use the `weighted_img()` function to add my annotated lanes to the original image.
![alt text][image8]



### 2. Potential shortcomings with this pipeline

One potential shortcoming would be what would happen when the input image is of a different resolution. The region of interest would cut out the wrong part of the image, as my pipeline does not scale the roi. The same thing applies if the camera would be mounted in a different angle or position.

Another shortcoming is that in between some frames the extrapolated lanes vary quite a bit, especially on the side of the dottet lane markings (the ones to seperate different lanes). This results in flickering of the extrapolated lanes, even they are still quite accurate close to the car, which could lead to instability if one would apply a steering controller based on this lane detection. Also in very few of the frames, the left line is not detected and there are some false detections instead.


### 3. Possible improvements to the pipeline

A possible improvement would be to implement scaling of the ROI depending on the input picture dimensions. As the ROI is defined by a Polygon, but for the lane extrapolation hardcoded values are used it would be better to acutally calculate the intersection of the lanes and the ROI Polygon.

Another potential improvement could be to further optimize the parameterization to extrapolate more stable lines. This could for example be implemented with averaging the detected lines over time - or in this case frames.
