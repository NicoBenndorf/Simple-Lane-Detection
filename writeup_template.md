# **Finding Lane Lines on the Road** 

## Writeup Template

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image2]: ./plot_saves/2_grayscale.jpg "Grayscale image"
[image3]: ./plot_saves/3_gray_select.jpg "Grayscale image filtered by intensity"
[image4]: ./plot_saves/4_gray_select_roi.jpg "Filtered image reduced to the relevant ROI"
[image5]: ./plot_saves/5_img_canny.jpg "Image after canny-edge detection"
[image6]: ./plot_saves/6_canny_blur.jpg "Image after canny-edge detection and blur to strengthen edges"
[image7.0]: ./plot_saves/7.0_hough.jpg "Unused output image of the hough line transform - to show detected line-segments"
[image7.1]: ./plot_saves/7.1_lane_img.jpg "Image of extrapolated lines that represent the detected lanes"
[image8]: ./plot_saves/8_image_annotated.jpg "Original image with annotated lanes"
---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 8 steps. First, I loaded the input imag. As next step I converted the images to grayscale. Third, I filtered the image by intensity with the inRange function to filter out pixels that are definetely to dark for being lane markings. The fourth step was setting a region of interest along the area of the image where the lane markings of our lane can be. 
As the fifth step, I used the provided canny() function to detect the edges of the lane markings. 
To improve the output of the canny edge detection I applied a gaussian blur of kernsel size 3 to strengthen the edges.
After the edge detection I used the hough transform for finding lines. For this sixth step I adapted the hough_lines() function and added the lines as a return value. 
In order to draw a single line on the left and right lanes, I used the draw_lines() function within my own extrapolate_lanes_image() function as the seventh step. This function orchestrates the call of some other functions that first seperate the left from the right lines. Next I call my extrapolate_lines() function seperately on the left and then the right lines. For extrapolating the lines I calculate the average slope, the average y-Axis intersection and with that the average intersection with my set ROI boundarys. With these average values and the roi intersections for the left and the right lane I use the draw_lines function to draw one single extrapolated line for each lane.
Last but not least in step eigth, I use the weighted_img() function to add my annotated lanes to the original image.

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]
![alt text][image7.0]
![alt text][image7.1]
![alt text][image8]



### 2. Identify potential shortcomings with your current pipeline
extrapolate to ROI hardcoded, flickering extrapolation/lanes, not scaling to different image resolutions and camera angle/view

One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
