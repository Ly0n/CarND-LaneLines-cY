# Finding Lane Lines on the Ground

Finding lanes lines is an important step in localisation of the car relative to the road.
As human driver uses the lane lines as an fundamental information for pathway of the road.
In a real world scenario depending only on visible lanes can be critical, but it a solid landmark that 
should be used when available.

## The image processing pipeline:

1. The first step is the converting from RGB to Grey colorspace, representing the intensity of the light in total. The following Gaussian filter calculates the mean intensity in a cluster of pixels. This reducaes noise influence by single pixels.
2. After that we filtere the image with a Canny Edge Detector, looking for gradients in the picture. The high threshold and low threshold define a bandwith of gradients that will pass the filter
3. Next we are masking the image with a region of interest(ROI), since we know in which are the lanes will apear. The mounting of the camera determents the area and pattern of the ROI. Thats why in a real world example the ROI should be connected to the extrensic and intrensic calibration of the camera. The ROI starts at the bottom of the image and ends at the skyline in the image. The output images should only remain with lines that are on the road.
4. Next in line is the hough transform that calculates the representation of the image in the hough space showing the slope and intercept of all the remaining lines in the image. The output is represented as a list of lines. Since we know, that the camera is looking straight into the driving direction, we can assign all the lines to the left or right lane. After that we use the line length in driving direction as weight for the mean left and right lanes. This will reduce the impact of smaller lines and increases the impact of longer lines we are looking for. As a result we get a mean slope and intercept for the left and right lane. We can now calculate and plot the left and right lanes ending at the skyline

## Potential shortcomings:

1. This approch will not hold for complex lane marks in city scenarios since we are only able to detect on lane in driving direction.
2. We do not use any model for the lane dimensions. Every visible line in the driving direction could be seen as lane.
3. The information of the last image have no influence on the next processing cycle. Useing statistic over multiple images can reduce the influence of outlier.
4. ROI should be defined by the camera calibration.
5. Lane model will fail in very curved roads

## Possible Improvements:

1. Tracking of the lane between multible images.
2. Using a lane model, since the marking of the lanes depends on standards.
3. Splines instead of straight lines let you detected curved lanes.
4. Calculate the ROI from the camera calibration
