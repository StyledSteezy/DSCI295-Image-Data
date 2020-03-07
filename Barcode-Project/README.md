# Barcode Detector Project
Bar codes are an early vision system has be come ubiquitous in every day life. The goal for this project is to build a system that can 
recognized bar codes in images.

You will start with the code I have written to decode EAN-13 barcode. The decodeEAN13 function takes a cropped image img (in the format 
openCV gives) of a horizontal barcode, a vertical offset y, a threshold t, and a boolean adaptiveNormalize that can probably always be True 
that will try to normalize the input based on a local min/max. The decodeEAN13 function will return a tuple of a boolean indicating if the 
barcode was successfully decoded and an array of the digits of the barcode. If unsuccessful the array will have debugging information as 
well. The ultimate goal is to have a tool that takes an image and gives the values and locations of all the barcodes in the image without 
any user intervention.

EAN-13 decoder code: 

![alt text](https://github.com/StyledSteezy/DSCI295-Image-Data/blob/master/Barcode-Project/starting-code.PNG "Starting Code")

# Project Requirements
- Write code to find barcode like things in an image that will produced cropped images of barcodes to try to decode with decodeEAN13.
- Evaluate the effectiveness of your solution.
- Write a detailed description of your work:
    - What you did.
    - What worked and didn't work.
    - How the techniques you use fit in our framework of understanding image data (Raster, Vector, and Functional representations, and the 
    transitions between these representations).
    
# Write up
Overall this project went very well. Although the final product is not perfect I was able to crop out the barcodes in some of my images. 
I was also able to explore some very interesting aspects of image data and find interesting things throughout this project. There were 
multiple different steps to complete this project. The first step was to take pictures with barcodes in them. And the final goal of this 
project was to write a program that will find the barcodes within the image and crop the section with the barcode.

After importing my 
image I converted it to grayscale. Once I made the image grey I wanted to use edge detection to find where the barcode was in the 
picture. This took some research. Because there are a couple of different ways of going about this I tried the top two that other people 
recommended online. I attempted to find the barcode with Canny Edge Detection and Mask Edge Detection. In the end, I found out that 
Canny Edge Detection worked better for me. Canny works by taking the image from the Sobel output. Thinning all the edges so they are 1
pixel wide and then outputting the edges from the image.

![alt text](https://github.com/StyledSteezy/DSCI295-Image-Data/blob/master/Barcode-Project/image-processing/greyscale-images.PNG "Grey Scale Images")

Once I was able to detect the edges within the image I drew lines over the image to try
and find where exactly the barcode was. I was able to do this with a for loop that would go
through the image and draw lines over the detected edges. Then the image had to be cropped
where the largest gap was between two lines. This was done with the crop function that Ryan
Ozzello made. Ryan helped me out a lot on this part of the program. To crop the image the
program would look at the first pixels in either the x-axis or y-axis. Because these are raster
images they are made up of pixels. Using the picture below as an example the function would go
across the very top line of pixels and mark all of the read lines. It was then able to pick the lines
that were farthest apart and crop the image and only output the barcode.

![alt text](https://github.com/StyledSteezy/DSCI295-Image-Data/blob/master/Barcode-Project/image-processing/detecting-lines.PNG "Detecting Lines")

The two parameters - 1237 and 2063 are the furthest apart out of all the lines on the
x-axis. So it will now crop everything that is not within that range out. If this was a vector image
the crop function would have to analyze either the x-axis or y-axis in a specific range instead of
the pixels to find the red lines.

And this is what the final result looks like:

![alt text](https://github.com/StyledSteezy/DSCI295-Image-Data/blob/master/Barcode-Project/image-processing/detecting-barcodePNG.PNG 
"Detecting The Barcode")

Now lets see some of the results of my images. The most interesting result out of my 10 images was the image of the chip bag.

![alt text](https://github.com/StyledSteezy/DSCI295-Image-Data/blob/master/Barcode-Project/barcode-images/IMAGE%233.jpg 
"Chip Bag")

As you can see the whole barcode is at a very strange angle because the bag is crinkled.
Although, my program was still able to pick out a sliver of the barcode. So essentially this
example was a success because all the barcode reader needs are just the width between all the
lines.

Here is the final result of this example- surprisingly it worked!

![alt text](https://github.com/StyledSteezy/DSCI295-Image-Data/blob/master/Barcode-Project/image-processing/chip-bag-barcode-detection.PNG "Chip Bag Result")

An example that did not work would be the image of the shoe box.

![alt text](https://github.com/StyledSteezy/DSCI295-Image-Data/blob/master/Barcode-Project/barcode-images/IMAGE%236.jpg 
"Shoe Box Barcode")

This image was going to be particularly hard to find the barcode because of the shadow
interfering and possibly because of all the black in the image? So I ran my program on this image
and the output was this:

![alt text](https://github.com/StyledSteezy/DSCI295-Image-Data/blob/master/Barcode-Project/image-processing/shoe-box-barcode-detection.PNG  "Shoe Box Barcode")

There are just too many lines drawn on this image. Even though it was able to crop out
the part of the image where the lines are farthest apart, it is still unreadable because there are just
too many lines and the sliver is too small to read. To fix this I would need to change the
threshold that decides on how many points will make a line. But if I were to change that, then it
might not work for the other images where it does work now. This was one of the troubles that I
ran into during this project.

Another trouble that I ran into was the error “max() arg is an empty sequence” that came
up on certain images. I believe that I was getting this error with some of my images because no
lines were ever being drawn on the image. And I think that that is because either my camera was
too poor and did not take clear enough pictures, or the shadows within the images were
interfering with the edge detection. Although, in the end, I was able to successfully find and crop
out the barcodes on certain images.
