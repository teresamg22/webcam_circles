# Exercise 1.4. Circle detection: webcam_circles
Circle detection from online webcam images
In the Git repository  https://github.com/beta-robots/webcam_circles

- Fork it
- Build the circle_detector example. (mkdir build & cd build & cmake .. & make)
- Play with it
- Find out information about how The Hough Transform works, and interpret and explain the behaviour when changing the parameters. Document your Readme.md and cite the used references.

The objective of Hough's is making it possible to group the points that belong to the edges of possible figures through a voting procedure on a set of parameterized figures.
Hough's transform algorithm uses a matrix, called an accumulator, whose dimension is equal to the number of unknown parameters of the problem. For example, to detect the existence of a line of the form y=mx+n the dimension of the accumulator would be two, either its representation in Cartesian coordinates (m,n) or polar coordinates (p,theta). The two dimensions of the accumulator correspond to the quantified values for (p,theta).
To build the accumulator it is necessary to discretize the parameters that describe the figure. Each cell of the accumulator represents a figure whose parameters can be obtained from the position of the cell.
For each point in the image, all possible figures to which that point may belong are searched. This is achieved by searching for all possible combinations of values for parameters that describe the figure (possible values are obtained from the accumulator). If so, the parameters of that figure are calculated, and then the position in the accumulator corresponding to the defined figure is searched, and the value in that position is increased.
The figures can be detected by searching for the accumulator positions with the highest value (local maximums in the accumulator space). The easiest way to find these peaks is to apply some form of threshold, but different techniques could give better results in different circumstances, determining where the figures are and how many there are.
Because of the errors that can be made detecting edges, there will be imperfections in the accumulator space, which can make it not trivial to find the correct peaks and therefore the appropriate figures.

To find circles using the Hough transformer, a three dimensional accumulator (a,b,r) is needed.
Then each dot in the image votes for the circumferences it might be in. Once this procedure is finished, the peaks in the accumulator are searched and with this the radius and the center of the circumference are obtained. If the radius were known beforehand, only a two-dimensional accumulator would be needed.

The parameters that has been use for the Hough transformer are the following ones:
- src_gray: Input image (grayscale)
- circles: A vector that stores sets of 3 values:  for each detected circle.
- CV_HOUGH_GRADIENT: Define the detection method. Currently this is the only one available in OpenCV
- HOUGH_ACCUM_RESOLUTION: The inverse ratio of resolution
- MIN_CIRCLE_DIST: Minimum distance between detected centers
- CANNY_EDGE_TH: Upper threshold for the internal Canny edge detector
- HOUGH_ACCUM_TH: Threshold for center detection.
- MIN_RADIUS: Minimum radio to be detected. If unknown, put zero as default.
- MAX_RADIUS: Maximum radius to be detected. If unknown, put zero as default.

In this .cpp it also uses a gaussian Gaussian filter to blur the image with this we can reduce the noise so we avoid false circle detection The parameters of the gaussian filter are:
 cv::GaussianBlur( gray_image, gray_image, cv::Size(GAUSSIAN_BLUR_SIZE, GAUSSIAN_BLUR_SIZE), GAUSSIAN_BLUR_SIGMA );

- src: input image; the image can have any number of channels, which are processed independently, but the depth should be CV_8U, CV_16U, CV_16S, CV_32F or CV_64F.
- dst: output image of the same size and type as src.
- Size(GAUSSIAN_BLUR_SIZE, GAUSSIAN_BLUR_SIZE): Gaussian kernel size. ksize.width and ksize.height can differ but they both must be positive and odd. Or, they can be zero’s and then they are computed from sigma* .
- GAUSSIAN_BLUR_SIGMA sigmaX: Gaussian kernel standard deviation in X direction.
                      sigmaY: Gaussian kernel standard deviation in Y direction; if sigmaY is zero, it is set to be equal to sigmaX, if both sigmas are zeros, they are computed from ksize.width and ksize.height, respectively; to fully control the result regardless of possible future modifications of all this semantics, it is recommended to specify all of ksize, sigmaX, and sigmaY.
- borderType: pixel extrapolation method.


References: 
https://en.wikipedia.org/wiki/Hough_transform7
https://docs.opencv.org/2.4/doc/tutorials/imgproc/imgtrans/hough_lines/hough_lines.html
https://docs.opencv.org/2.4/doc/tutorials/imgproc/gausian_median_blur_bilateral_filter/gausian_median_blur_bilateral_filter.html
https://docs.opencv.org/2.4/modules/imgproc/doc/filtering.html?highlight=gaussianblur#gaussianblur
