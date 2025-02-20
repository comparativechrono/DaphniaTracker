# DaphniaTracker
Code for tracking Daphnia from images captured from microscoPI work.

What the beta version does:

The analysis is designed for a series of images to track the vertical movement of Daphnia in T25 flasks over time.

The very first image is used as a static background reference. From this image, we define a specific Region of Interest (ROI) that corresponds to the area of the flask where the Daphnia are present. By cropping the image to this ROI, the analysis focuses only on the relevant part of each frame, which makes the subsequent steps more efficient and accurate.

For each subsequent image, the method employs dynamic background subtraction. This means that the program subtracts the previous image (the prior state of the ROI) from the current one, thereby highlighting changes that are likely due to the movement of the Daphnia. To further enhance the image and reduce noise, a Gaussian filter is applied, which smooths out minor fluctuations that could be mistaken for movement.

Once the changes are isolated, the program detects particles in the subtracted image. It does this by converting the image into a binary format (where the pixels are either “on” or “off” based on a threshold) and then labels connected regions that represent potential Daphnia. Only the larger regions—those that exceed a minimum size—are considered valid. The centroids (central coordinates) of these regions are then calculated as estimates of the Daphnia positions. Since we are particularly interested in their vertical movement, the y-coordinates of these centroids are converted into percentages relative to the height of the ROI. This conversion makes it easier to compare positions across different frames, as the values are standardized between 0% and 100% (with 100% corresponding to the top of the ROI).

The detected positions for each image are stored in a structured table. From this table, we compute the average vertical position (mean), the variability (standard deviation), and the precision of our estimate (standard error of the mean) for each time point. The time associated with each frame is also calculated, converting frame numbers into minutes and hours, while allowing for an initial adjustment of the start time.

The notebook produces several plots to visualize the data. One plot shows the mean vertical position of the Daphnia over time with error bars representing the standard error. Another plot incorporates a sliding average to smooth out short-term fluctuations, making longer-term trends clearer. A more comprehensive plot then overlays the light and dark phases of the diurnal cycle onto the data, highlighting how the Daphnia's vertical distribution might correlate with changes in lighting conditions. 

Variables that need to be adjusted is time at start (set at 2.5 here), and the photoperiod duration. Additioanlly, the position of the ROI will need adjusting. Other than that, the notebook is automated.
