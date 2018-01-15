# **Finding Lane Lines on the Road**

#  **Abdullah Ali** 



### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)
[image1]: ./GrayScale_solidWhiteRight.jpg "GrayScale_solidWhiteRight.jpg"

[image2]: ./HSLMASK_solidWhiteRight.jpg "HSLMASK_solidWhiteRight.jpg.jpg"

[image3]: ./GuassianBlur_solidWhiteRight.jpg "GuassianBlur_solidWhiteRight.jpg"

[image4]: ./Canny_solidWhiteRight.jpg "Canny_solidWhiteRight.jpg"

[image5]: ./Interest_solidWhiteRight.jpg "Interest_solidWhiteRight.jpg"

[image6]: ./Output_solidWhiteRight.jpg "Output_solidWhiteRight.jpg"

[image7]: ./test_images/solidWhiteRight.jpg "solidWhiteRight.jpg"
---

### Reflection

### 1. Describing the pipeline.

We have 6 parts of this pipeline: Grayscale, HSL Mask, Canny Edge detection, Region of interest, Hough, Extrapolate

we will follow the transformation of one of this image 


 ## 1
 
 
	First convert the image to gray scale 
 
 ![alt text][image1] 
 
 
 ## 2
 

	Then we use  cv2.cvtColor(image, cv2.COLOR_RGB2HLS) and set lower and upper white and yellow boundaries to apply white and yellow masks on the image
 to obtain the HSL mask we first use bitwise OR on the just obtained white and yellow masks
 then we use bitwise AND on the result and the gray scale to obtain 
 
  ![alt text][image2] 
  
  
 ## 3
 
	To get to the Canny edge detection we first need to apply Guassian blur to our HSL mask 
	
	 ![alt text][image3] 
	 
	 when its done we can now use it in cv2.Canny(blur_gray, low_threshold, high_threshold)
	 
	 to get 
	 
	 ![alt text][image4] 	 
  
## 4

	Now we have to create the region of interest in order to apply Hough, to do that we will use the result from the canny edge detection and setting up the area that we will be using, by masking all useless regions in the image 
	
	
    rows, cols = edges.shape[:2]
    bottomLeft = [cols*.1, rows*.97]
    topLeft = [cols*.4, rows*.6]
    topRight = [cols*.6, rows*.6]
    bottomRight = [cols*.9, rows*.97]
    vertices = np.array([[bottomLeft,topLeft,topRight,bottomRight]], dtype=np.int32)
    masked_edges = region_of_interest(edges, vertices)
	
	  ![alt text][image5] 
 
## 5
 
	Now using the masked image created before we can call  cv2.HoughLinesP(img, rho, theta, threshold, np.array([]),minLineLength=min_line_len,maxLineGap=max_line_gap)

	and obtain the information needed to later draw the lane lines
	
  
## 6

	Now we use the information from the Hough to draw our lines, we use  draw_lines(line_image, lines, color=[240, 0, 0], thickness=4)
	where line_image is a blank that we create, and lines is what the Hough function returns 
	
	after the draw lines function is done, we get the end result 
	
 ![alt text][image6]	
	
	
---
## the challenge video 

 applying the pipeline on the video gives us the same result as the images 

### 2. Identify potential shortcomings with your current pipeline

when running the pipeline on the video there is a noticeable twitch to the lines when the lanes curve 



### 3. Suggest possible improvements to your pipeline

we need to find a way to make better sense of the lane curves so that it does not mess the drawing process that leads to the twitch observed
"# Udacity_Project1_Finding_Lanes" 
