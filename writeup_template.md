# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

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

The pipeline consisted of the following steps:

    1. Apply a grayscale filter to the original image. This simplifies our algorithm by allowing us to only focus 
    on bright/dark spots and in particular the gradient - the rate of change in brightness to detect the lane edges.

    2. Apply a gaussian filter to suppress noise and spurious pixel values. 

    3. Apply the canny algorithm with low and high threshold parameters to convert image into binary with 
    white pixels tracing out edges and black elsewhere.
    
    4. Apply a region mask with specified vertices to only include edges in the general area of the lanes.

    5. Apply a Hough transform to convert pixel edges into lines.

    6. Finally, draw the Hough lines onto the original image. 

    7. I modified the draw_lines function to convert the many individual short lines identified by the Hough transform into
    single left and right lines to map the full extent of the lanes. This was accomplishd via identifying the left and right lines by their slope and calculating a single slope and intercept to describe the line and extrapolating to the 
    full length of the lane. I also excluded more horizontal lines with slopes within -0.3 to 0.3, which made the algorithm more robust for the challenge video.  

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

There are a few potential shortcomings with the current pipeline. In particular, the algorithm assumes that each of the left and right Hough lines will have a similar slope value and therefore we can extrapolate the full extent of the lane. In the case of very curved lines, this assumption will no longer hold. 

Also, as seen with the challenge video, any unexpected intrusions in the video, such as the blurred bottom portion, could cause issues with region masking area. Any unexpected changes in the camera mounting and position will of course cause issues.


### 3. Suggest possible improvements to your pipeline

One improvement would be to determine if the road is straight or curved. If it is curved, we would have to change our model by which we extrapolate and plot the full length of the lane. Each of the Hough lines will no longer have similar slopes but will vary as we proceed through the curve. Instead of relying on a linear regression fit, we could try polynomial fits that would better represent the curves. 

Also, as seen with the challenge video, sudden changes in brightness within the lane itself caused spurious Hough lines across the lane. Filtering out lines with more horizontal slopes made our algorithm more robust to such changes, but perhaps rather than using a quadrilateral region mask, we can have two region masks that would only encapsulate the left and right lanes solely. 

More image and video tests would help discover edge case scenarios we did not consider previously. 
