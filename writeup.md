# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)


[image0]: ./examples/laneLines_thirdPass.jpg "LaneLines"

[image1]: ./examples/grayscale.jpg "Grayscale"

[image2]: ./examples/line-segments-example.jpg "Line Segments"

---

### Reflection

### 1. Describe your pipeline.

 1. *Convert to Grayscale*:

    - Conversion from RGB to Gray scale is necessary to reduce the complexity in the operations we apply on a image.
    - This will return an image with only one color channel.
    - Faster computation.


    ![alt text][image1]

 2. *Gaussian Smoothening Filter*:

    - Gaussian Smoothening filter in 2D convolution operation that is used to remove noise and blur from the image.
    - Gaussian filtering is done by convolution each point in the input array with a gaussian kernel (size= 5) and summing them all to produce an output array.

 3. *Canny Edge Detection*:
    
    - Canny edge detector is an optimal edge detector. It takes as input a gray scale image, and produces as output an image showing the positions of tracked intensity discontinuities.
    - Canny edge detector works in multiple stages:
      1. First of all the image is smoothed by Gaussian convolution.
      2. Find the intensity gradients of the image.
      3. Apply non-maximum suppression to get rid of spurious response to edge detection.
      4. Apply double threshold to determine potential edges.
      5. Track edge by hysteresis: Finalize the detection of edges by suppressing all the other edges that are weak and not connected to strong edges.
    - Canny edge detector takes in two type of threshold, low threshold and high threshold (200 and 250 in my case). For those pixels whose intensity lies between low threshold and high threshold, they are being eveluated to be edge pixel if and only if their connected neighbouring pixel is edge pixel.

 4. *Region of Interest (ROI)*

    - Depending on the camera placement we define the ROI. In our case the camera is placed such that the lanes are at the bottom half of the image.
    - At a particular point the lanes appears to be converging (Also called vanishing point). We defined the Trapezoid just below the vanishing point (height *0.6 in my case).

 5. *Hough Transform*

    - The Hough transform is a technique which can be used to isolate features of a particular shape within an image (Lines in our case)
    - I have classified left lanes and right lanes based on the slope of the line (positive slope right edge else left edge) obtained from Hough transform.
    - Lines with slopes between a minimum value and maximum value is choosen which corresponds to the lane dimention. 

      ![alt text][image2] 

 6. *Average/Extrapolate lanes*
    - Different approaches can be applied. We can use polyfit function to fit a line of ``y = mx + c`` fromt he set of lines obtained from hough transform. 
    - I have choosen to take average of slope and interception from the hough transform lines and extrapolated before the vantage point by reconstruction the line equation (Drawline function).

    ![alt text][image0]

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be when there is no lane markings on the road the approach fails. This is due to the obvious fact that the edges are not detected. This scenario occurs due to occlusion or obtruction on the road.

Another shortcoming could be due to the minimum and maximum slope filtering. We can observe this in the optional challenge video when the road curvature increase the slopes are filtered and there is no lane output.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to to use polyfit when the lane curvature increases and not apply slope filtering.

Another potential improvement could be to use moving average filter for smooth lane detction with highter degree polynomial rather and straight lane detection.

