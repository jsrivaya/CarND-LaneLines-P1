#**Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

![alt text][image1]
![alt text][image5]

[//]: # (Image References)

[image1]: ./test_images/solidWhiteCurve.jpg "Original"
[image2]: ./final_images/solidWhiteCurve-blur.jpg "Blur"
[image3]: ./final_images/solidWhiteCurve-canny.jpg "Canny Image"
[image4]: ./final_images/solidWhiteCurve-mask.jpg "Masked Image"
[image5]: ./final_images/solidWhiteCurve-final.jpg "Final Image"

---

3### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps.

- Convert Image to Grayscale

  ```
  gray = grayscale(image)
  ```

- Getting a gaussian approx
We use a kernel size of 3. The reason for this is that we don't want the picture to get too blur (kernel > 5)

    ```
    kernel_size = 3
    blur_gray = gaussian_blur(gray, kernel_size)
    ```
    
![alt text][image2]

- Getting canny image
Decided to use a triangle as a region of interes, being the apex at the 58% of the image height. The reason for this is that we consider the horizon (~ 58% of the image height) placed in the infinite

![alt text][image3]

- Define region of interest

    ```
    left_bottom = [0, canny_image.shape[0]]
    right_bottom = [canny_image.shape[1], canny_image.shape[0]]
    apex = [canny_image.shape[1] * .50, canny_image.shape[0] * .58]
    ```

- Find lanes using Hough Transform
Here are the values used to find lines of interest
    
    ```
    rho = 1
    theta = np.pi/180
    threshold = 25
    min_line_length = 5
    max_line_gap = 2
    ```

- Drawing Lines on the Image

In order to draw a single line on the left and right lanes, I modified the draw_lines() function.
To draw a line we need two points but we can also do it by knowing a point and the slope of the line. (y - y1) = m (x - x1)
I calculated the average slope of all the lines got from the hough transform. To know if a line is facing left or right we only have to check if the slope is positive or negative. If the slope is positive the line is part of the left line. If the slope is negative the line is part of the right line. How do we get a point that is part of that line? We average all 'x' and 'y' of all points of the hough lines. That way we calculate the average center.

We don't want to draw infinite lines so we have to find the top and bottom points of the line. We choose as top point one that corresponds with the y=58% of the image height, and the bottom point will be one that corresponds with the y=Image_height. Since we have the equation of the line, we can find the 'x' for each of those 'y' and then draw the line.

![alt text][image5]

###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when we find two lines very close together line when two lanes merge 


###3. Suggest possible improvements to your pipeline

A possible improvement would be to accept larger ranges of colors for the lines

Another potential improvement could be to improve the region of interes
