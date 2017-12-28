# **Finding Lane Lines on the Road** 


[//]: # (Image References)

[canny]: ./writeup_images/canny_edges.jpg
[masked]: ./writeup_images/masked_lanes.jpg
[detectedLanes]: ./writeup_images/hough_lines_overlay.jpg
[detectedLanesWithConnection]: ./writeup_images/hough_lines_with_connection.jpg
[solidYellowCurve2]: ./writeup_images/solidYellowCurve2.jpg

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of following steps.

1. In first step, I converted the image to grayscale. 

2. Then I ran gaussian blur, to smoothen the image. I tried different kernel sizes (3, 5, 7). Kernel size of 5 yielded best results for me.

3. I then ran canny algorithm to detect the edges using gradient. In this case I used already provided helper method. At this stage image should be all black except for edges.

4. Now, I need to define a region of interest. The idea here is to remove all edges in image from step #3 except for the lanes we want. 
The helper method `region_of_interest` helps us here. 
All I need to do was define a polygon around the lane we are seeking to identify.
I faced a challenges here which is to ensure polygon is consistent for all test images. 
After a while I realized, even though we are detecting left lane in one image and right lane in another,
our pipeline does not need to worry about it. Because images were provided in such a way that lane of interest is
in a similar region (w.r.t to frame of reference) in all images.

5. This step included chosing proper parameters for polar coordinates (rho and theta) and use helper method to apply hough transform and drawlines.

6. In drawlines method, as instructed I made change to connect dotted lines together.

6. Final step is to overlay detected lane on image and save it.

![after running canny algo][canny]
![masked image][masked]
![HoughLines][detectedLanes]
![HoughLines With Connections][detectedLanesWithConnection]
![Overlay on Road][solidYellowCurve2]



### 2. Identify potential shortcomings with your current pipeline


I noticed that for most part choosing lanes was planing around with image and finding good polygon. But I am not sure, this will scale for arbitrary road (with curves etc).
Even for one of the videos (optional challenge), we have to change pipeline for it.
 

### 3. Suggest possible improvements to your pipeline

In line with my previous statement, I would like to find a way of automatically detecting lanes without having
to manually overlay a polygon.

Also, to connect dotted lines using average does not seem robust. I researched about this in internet
and saw various people using slope and intercept to remove outliers and then find average intercept and slope.
I tried this in my code but did not get good results. I need to fine tune it further.