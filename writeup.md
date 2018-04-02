# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline (get_lane_lines in the code) has the following stages:
1. Convert to grayscale
2. Add Gaussian blur
3. Use Canny to detect edges
4. Use a quadrilateral region of interest to mask off the image
5. Find lines using Hough Transform
6. Use a heuristic to select only the two relevant lines:
    * This heuristic first sorts the lines by length
    * It next selects a line with a positive slope and one with a negative slope, with a length as long as possible
7. Cleanup the lines:
    * Ensure both lines hit the bottom of the screen, using the equation for a line
    * Find the intersect of the lines and make sure the line does not go above it.

I did not have to modify the draw_lines function. This was taken care off by Step 7.

### 2. Identify potential shortcomings with your current pipeline

My heuristic in step 6 has several shortcomings. 
1. I transform the lines detected in 5 to Euclidean line equations, with a and b parameters in y=ax+b. As with Hough Transforms, polar coordinates should be used in order to be able to represent vertical lines. Lanes that are exactly vertical in the image, though rare, will not be able to be represented with Euclidean line equations.
2. Another assumption is that are looking for a line with a positive and a negative slope (parameter 'a'). There could be situations where this assumption is no longer valid and slopes have the same sign (e.g., a very sharp bend or U-turn).

Further, I noticed that the lines I detect in step 5 run longer than expected. This is especially through for the lines that matter: the lane lines. They seem to run beyond the intersect, which doesn't make sense. My dirty solution is to introduce step 7, which cleans up everything beyond the intersection.

### 3. Suggest possible improvements to your pipeline

One idea for a better heuristic is draw an imaginary vertical line in the middle of the windshield. Next, we sort the identified lines by length again, but only take those that are closest to the imaginary vertical line. This avoids the shortcomings of the current heuristic. In addition, it might help solve the "Optional Challenge". I noticed, in the optional challenge, that sometimes other lane lines were selected.
