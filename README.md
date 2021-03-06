# **Finding Lane Lines on the Road** 

[//]: # (Image References)

[gray]: ./pipeline_images/gray.jpg "Grayscale"
[dir]: ./pipeline_images/dir.jpg "Direction"
[mag]: ./pipeline_images/mag.jpg "Magnitude"
[grad]: ./pipeline_images/grad.jpg "Gradient"
[left]: ./pipeline_images/left.jpg "Left"
[right]: ./pipeline_images/right.jpg "Right"
[hough]: ./pipeline_images/hough.jpg "Hough Lines"
[final]: ./pipeline_images/final.jpg "Final"

---

### Reflection

### 1. Pipeline:

1. Convert the image to grayscale.
![Grayscale][gray]
2. Get the gradient direction and magnitude at each pixel using Sobel gradients.
![Direction][dir]
![Magnitude][mag]
3. Filter the image on gradient magnitude and direction.
    a. Magnitude lowerbound chosen such that only lines with sufficient gradient change (like a lane) are chosen.
    b. Direction bounds chosen so as to filter out horizontal lines.
![Gradient][grad]
4. Blur the output of the gradient filter with a Gaussian
5. Create a region of interest to mask out where the lanes will likely not be found.
   a. create a mask for the left and right sides.
![Left][left]
![Right][right]
6. For each hough line, obtain the line parameters (x = m * y + b) and then average them. Overlay these averaged lines onto the image.
![Hough Lines][hough]
![Final][final]

### 2. Potential Shortcomings:

1. The pipeline could do a better job of filtering out horizontal lines.
2. Using a left and right mask is likely not as robust as determining the "center" of the image using the hough lines themselves. If the car was to the right side of the road, the right side of the image might end up not containing a lane. In which case, this pipeline would incorrectly report an erroneous lane for the right side.
3. Shadows and weather changes present a considerable challenge for lane detection. Methods would need to be employed to make the pipeline robust to illumination changes as well.
4. The algorithm makes no use of the fact that the lanes are not completely independent. In fact, the lanes are coupled by the fixed width between them. Hence information on the position of one lane could be used to inform the position of the other lane.
5. No tracking is employed.

### 3. Possible Improvements:

1. Some method of incorporating past measurements so as to "smooth" the detection and make it more reliable. Either a moving ROI which is centered at the previous detection, or some logic which only permits the lane parameters to change within a certain amount from one frame to another.
2. Add some more code to reject horizontal lines before averaging.
3. Instead of a simple average, do a weighted average based on the number of votes each line received.
4. Try a different line-fitting algorithm (RANSAC).
