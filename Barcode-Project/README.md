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
'''python
import cv2
import numpy as np
from matplotlib import pyplot as plt

from wand.image import Image
from wand.color import Color
with Image(filename='200px-Example_barcode.svg.png') as wimg:
    with Color('#fff') as white:
        with Image(width=wimg.width, height=wimg.height, background=white) as back:
            back.save(filename='back.png')
            back.composite(wimg,0,0)
            back.save(filename='test.png')
            
img = cv2.imread('test.png')

px = img[1,10]
print(px)
[255 255 255]

plt.imshow(img)
'''
