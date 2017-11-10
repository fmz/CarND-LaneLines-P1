# **Finding Lane Lines on the Road** 


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

My pipeline consisted of 5 steps.

1. Convert the image to grayscale and apply a gaussian blur to it.
2. Now the image is ready for Canny edge detection, so use that.
3. Extract the edges that are of interest. In our case, the ones that lay in a trapezoid in-front of the car (the size of the trapezoid was chosen by trial and error).
4. Apply Haugh transform, extracting lines from the edge information we got in <2>.
5. Extrapolate the lines, discard the ones that aren't needed, and draw them out.

Particularly draw_lines() had to be modified as follows: First, given the lines we got from the haugh transform, discard the bogus lines. It turns out that lines with a slope that's too low or too high are not needed. Also, lines that don't hit the bottom edge of the picture when extrapolated are also probably bogus. Next, extrapolate each line from the bottom of the image to just below the mid-point, you'll notice that we'll get 2 sets of points: points at the low-left of the image and the low-right of the image, both of these sets connect to a corresponding set of points around the middle of the image. Basically, we need to figure out which sets correspond to the left lane and which correspond to the right.
Once we know our sets, we average them out, get one line per lane, draw it, and voila!

This pipeline also does a good job for the challenge. The one extra thing to do was to amplify the yellow color, because it gets faded at times.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the lines are too curvy or the camera is too shakey.


### 3. Suggest possible improvements to your pipeline

Fit a quadratic polynomial, discard more kinds of bogus points (like very close cars or random lines on the road).
