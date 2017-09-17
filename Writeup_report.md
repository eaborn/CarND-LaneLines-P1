
# **Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve_segment_output.jpg
[image2]: ./test_images_output/solidWhiteCurve_connected_output.jpg

---

### Reflection

### 1. Describe the pipeline.

My pipeline consisted of 5 steps:
1) convert the images to grayscale.
2) use the Canny Edge Detection method to get the edges.
3) set a region mask to only get the edges within the region.
4) use the hough transform to get the solid lane line within the region.
5) draw the lines onto the original image.
    
#### Steps to draw two single lane lines
HoughLinesP function will return many segment lines.
    Each line: y = m * x + b.

In order to draw a single line on the left and right lanes, I **created the hough_lane_lines() function by setting ±0.5 as threshold to distinguish the left lane line and right lane line**. For each lane, I will first sum up the segment lines's parameter (m and b), and then average them to get the mean m and b. By using the **generate_line()** function I can get the two end points: (x1, height), (x2, 0.6019*height) of the line. So that we can get the single line of the left and right lanes by calling **draw_lines()** funciton

#### The lane line results are shown below (using the solidWhiteCurve.jpg as an example): 

![segment lane line][image1]
![connected lane line][image2]



### 2. Identify potential shortcomings with the current pipeline


One potential shortcoming would be our find_lane_line() method depends on Canny Edge Detection, in which we first get the gray image from RGB iamge. If we cannot see any lane line in gray image, the method won't work. 

Another shortcoming could be that when applying the method to the **challenge video**, it is hard to find the lane lines. We can see that sometimes in last frame, it is doing well. However,  in current frame, it will detect the wrong lane line. 

### 3. Suggest possible improvements to the pipeline

My solution for the **RGB to Gray** problem is to use **cv2.COLOR_BGR2Luv** instead of **cv2.COLOR_BGR2GRAY** or **cv2.COLOR_RGB2GRAY** to get the image

My solution for the **challenge video** problem is to set a calibration method - calib_line() function. By applying the calibration function, we will use the lane line in last frame if the lane line in current frame is too far from the lane line in last frame. It can improve the result in some degree but sill there are a lot more improvement that needs to be done to get better result such as applying the machine learning method.

