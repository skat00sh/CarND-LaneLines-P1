# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

1. First step was to detect edges in the whole image using canny image detection. Canny Edge Detection works in 3 steps braoadly: 
  
  a: Convert the images to grayscale 

![gray](https://github.com/skat00sh/CarND-LaneLines-P1/blob/dev_P1/examples/blur.png?raw=true)

  b: Apply Gaussian Blur (I used kernel of size=5) to smoothen the image
  
 ![blur](https://github.com/skat00sh/CarND-LaneLines-P1/blob/dev_P1/examples/blur.png?raw=true) 
 
  c: On the image obtained above apply the canny image detection, by defining lower and higher thresholds (I used low threshold=80 & high_threshold=130). This completes the first step of detecting edges and we obtain the following result
  
  ![edge](https://github.com/skat00sh/CarND-LaneLines-P1/blob/dev_P1/examples/edge_detected.png?raw=true)
  
2. Now we need to find the lines corresponding to the lanes on the road. So first we define a region of interest and then apply Hough Transform to get a result as shown below

![hough1](https://github.com/skat00sh/CarND-LaneLines-P1/blob/dev_P1/examples/line-segments-example.jpg?raw=true)

3. Now we need to extrapolate the lines and smoothen so that we can get just two straight lines instead of the segmented lines we see in theprevious image. We wish to get a result as shown below:

![hough2](https://github.com/skat00sh/CarND-LaneLines-P1/blob/dev_P1/examples/laneLines_thirdPass.jpg?raw=true)

In order to do that, first we need to separate the left lane lines from the right ones. We do this separation on the basis of slope of lines. Left lanes will have a positive slope and right ones will have a negative slope. Now that we have slope of evey lane line and we have separated them as well, we find out an average or mean slope (whichever you prefer, I chose average slope). Not we need to find the average or mean left and right intercept. we store all left_y_coordinates corresponding to lines from hough transform and similarly for left_x_cordinates, right_y_cordinates and right_x_cordinates. Now we find average/mean for all find left and right intercept using the equation c = y - mx, wher y and x are average/mean left and right coordinates.
Now that we have average/mean slopes and intercepts we find the x-cordinates using x = (y-c)/m. Here we take advantage of the fact that, as we described a region of interest earlier, we know that y-cordinate can only have 2 values. y_min = minimum y cordinate among all the possible lines and y_max = the maximum possible value for y-cordinate i.e height of image. (keep in mind that y cordinate starts from top of the image and goes towards the bottom as in maximises, so y_min will be somewhere around the centre of the image and y_max bottom of the image)


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there would not be good lighting

Another shortcoming could be when the camera angle is constantly changing

Also, as in Challenge video, when the car is turning


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to include a method that could deal with dim-lighted images and could work properly even in presence of shadow

Another potential improvement could be to detect lanes while turning as in almost every frame lane in a curved line instead of a straight one. Probably using snakes algorithm might help, but not sure!
