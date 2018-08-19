# **Finding Lane Lines on the Road** 

[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteRight.jpg "Processed Image"
[video1]: ./test_videos_output/solidWhiteRight.mp4 "Solid White Right"
[video2]: ./test_videos_output/solidYellowLeft.mp4 "Solid Yellow Left"

---

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

1. Gray scale the image
2. Gaussian Smoothing
3. Canny Edge Detection
4. Masked Edges
5. Hough Transform – To identify lines in the ROI

In order to draw a single line on the left and right lanes as follows:
1. Considered the slope values only in the range {(-0.8 to -0.5) and (0.5 to 0.8)} seperated the left and right slope values by applying following condition
```
if ((m<0.8 and m>0.5)or(m>-0.8 and m<-0.5)): 
        #co-ordinates belong to the left lane or else right lane
        if (x1 < (img.shape[1]/2) and x2 < (img.shape[1]/2)) -> (x,y) 
```
2. Calculated c value in the equation $y=mx+c$ for individual point and seperated them into right and left array indiviually
3. Calculated average m and c for right and left respectively
4. Extrapolated the lines upto the vertices.

![alt text][image1]

Here's a [Solid White Right Line Video][video1] and [Solid Yellow Left Line Video] [video2]


### 2. Identify potential shortcomings with your current pipeline


1. Not able to compensate for the curvature of the road
2. In very high or low lighting condition it wont be able to identify the lane lines in such scenario it will perform better when canny edge detection is applied to image converted in HSV format
3. Lines are too shaky


### 3. Suggest possible improvements to your pipeline

1. Use of $y = ax^2+bx+c$ for the curvature of the road instead of $y=mx+c$
2. Canny edge detection applied to HSV format image
3. Shaky lines can be eliminated by first eliminating the outliers $(mean - 2*std. dev.)$ and later by calculating weighted average for the slope and the constant 

$$Slope \ (m_i) = \frac{(y2-y1)}{(x2-x1)}$$

$$Length \ (l_i) = \sqrt{(y2-y1)^2+(x2-x1)^2}$$

$$Weighted\ Average\ Of\ Slope = \frac{\sum_{i=1}^{n} {l_i*m_i}}{\sum_{i=1}^{n} {l_i}}$$