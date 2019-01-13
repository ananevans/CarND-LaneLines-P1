# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve.png "Solid white curve"
[image2]: ./test_images_output/solidWhiteRight.png "Solid white right"
[image3]: ./test_images_output/solidYellowCurve2.png "Solid yellow curve"
[image4]: ./test_images_output/solidYellowCurve.png "Solid yellow curve"
[image5]: ./test_images_output/solidYellowLeft.png "Solid yellow left"
[image6]: ./test_images_output/whiteCarLaneSwitch.png "White car lane switch"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of 5 steps. 
1. I converted the images to grayscale. 
2. I used gaussian blur to reduce image noise. 
3. I used the Canny algorithm to find the edges in the image. 
4. From this image, I kept only the part in front of the car where the lanes are likely to be, by using masking within a region of interest. I used a trapezoid with vertices `(150,539), (390, 350), (600, 350), (900, 539)`.
5. The next step is to use Hough algorithm to detect lines. Because some broken white lines have a large distance between the continous parts, I lowered the min_line_len pararameter to 20. 

Finally, I modified `draw_lines` to draw a single line on the left and right lanes. I grouped the lines from the previous step into ones with positive slopes and ones with negative slope, excluding lines almost horizontal and vertical lines. I used the `cv2.fitLine` method to find the left and right lanes.

Below are the processed images from the test folder:
![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]


For the challenge video, I changed the region of interest for masking and instead of drawing the best fit line through the positive and negative slope line points, I used `cv2.polylines`. When the left yellow line is too faded, my implementation can't find it.


### 2. Identify potential shortcomings with your current pipeline

The algorithm above does not perform well when the lines curve sharply. A line is not a good approximation.

Another issue are other lines detected in the vicinity of the lanes. I excluded the almost horizontal ones, but if cars or shadows are in the region of interest, then some extra lines with an acceptable slope will make the estimated line to be at a small distance from the actual lane line.

### 3. Suggest possible improvements to your pipeline

Possible improvments are: give higher weight to lines at the bottom of the picture, use information about car camera, car size and size of typical lanes to reduce the region of interest, mask the cars on the road. 
