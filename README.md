# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

[![Live Demo](https://raw.githubusercontent.com/Ly0n/CarND-LaneLines-cY/master/video_out/solidWhiteRight.jpg)](https://raw.githubusercontent.com/Ly0n/CarND-LaneLines-cY/master/video_out/solidWhiteRight.mp4)


Finding lanes lines is an important step in localization of the car relative to the road.
As human driver uses the lane lines as an fundamental information for pathway of the road.
In a real world scenario depending only on visible lanes can be critical, but it a solid landmark that 
should be used when available.

## The image processing pipeline:

1. The first step is the converting from RGB to Grey color space, representing the intensity of the light in total. The following Gaussian filter calculates the mean intensity in a cluster of pixels. This reduces noise influence by single pixels.
2. After that we filter the image with a Canny Edge Detector, looking for gradients in the picture. The high threshold and low threshold define a bandwidth of gradients that will pass the filter
3. Next we are masking the image with a region of interest(ROI), since we know in which area the lanes will appear. The mounting of the camera determines the area and pattern of the ROI. That's why in a real world example the ROI should be connected to the extrinsic and intrinsic calibration of the camera. The ROI starts at the bottom of the image and ends at the skyline in the image. The output images should only remain with lines that are on the road.
4. The next filter is the hough transform that calculates the representation of the image in the hough space showing the slope and intercept of all the remaining lines in the image. The output is represented as a list of lines. Since we know, that the camera is looking straight into the driving direction, we can assign all the lines to the left or right lane by the sign of the slope. After that we use the line length in driving direction as weight for the mean left and right lanes. This will reduce the impact of smaller lines and increases the impact of longer lines we are looking for in driving direction. As a result we get a mean slope and intercept for the left and right lane. We can now calculate and plot the left and right lanes ending at the skyline.

## Potential shortcomings:

1. This approach will not hold for complex lane marks in city scenarios since we are only able to detect lanes in driving direction.
2. We do not use any model for the lane dimensions. Every visible line in the driving direction could be seen as lane.
3. The information of the last image have no influence on the next processing cycle. Using statistic over multiple images can reduce the influence of out outlier.
4. ROI should be defined by the camera calibration.
5. Lane model will fail in very curved roads.
6. Thresholds for candy filter depend on a fixed camera setting.

## Possible Improvements:

1. Tracking of the lane between multiple images.
2. Using a lane model, since the marking of the lanes depends on standards.
3. Splines instead of straight lines let you detected curved lanes.
4. Calculate the ROI from the camera calibration.
5. Use AI to extract the lane marking.
6. Use mono SLAM to extract 3d points from lane features.
8. Match your data with global map lane data.
9. Put all together into a big kalman filter.
