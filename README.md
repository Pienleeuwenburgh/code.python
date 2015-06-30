# code.python
Excellentie programma - Geschreven code in Python


# import required modules (plug-ins)

import sys
import time
import Adafruit_MPR121.MPR121 as MPR121
import cv2
import numpy as np


# Load all images 

img_start = cv2.imread('./images/confused.jpg',1)

img0 = cv2.imread('./images/im0.jpg',1)
img1 = cv2.imread('./images/im1.jpg',1)
img2 = cv2.imread('./images/im2.jpg',1)
img3 = cv2.imread('./images/im3.jpg',1)
img4 = cv2.imread('./images/im4.jpg',1)
img5 = cv2.imread('./images/im5.jpg',1)
img6 = cv2.imread('./images/im6.jpg',1)
img7 = cv2.imread('./images/im7.jpg',1)
img8 = cv2.imread('./images/im8.jpg',1)
img9 = cv2.imread('./images/im9.jpg',1)
img10 = cv2.imread('./images/im10.jpg',1)
img11 = cv2.imread('./images/im11.jpg',1)


# put all info images in a list

visuals = [img0, img1, img2, img3, img4, img5, img6, img7, img8, img9, img10, img11]


# display some info in the terminal window

print 'Fishual presentation started!'
print 'To stop the program: press Ctrl-C to quit.'


# create a window and display the start screen

cv2.namedWindow('SensShow', cv2.WINDOW_NORMAL)
cv2.imshow('SensShow',img_start)
cv2.waitKey(1000)

# Create MPR121 instance and initialize communication with MPR121

cap = MPR121.MPR121()
if not cap.begin():
    print('Error initializing MPR121.  Check your wiring!')
    sys.exit(1)


# Main loop to print a message every time a pin is touched.


while True:
    
    # display start screen as long as no pins are touched
    if not cap.touched():
	# print 'Not touched anything'
	cv2.imshow('SensShow',img_start)
        cv2.waitKey(100)  
    else:
        # Check each pin's state to see if it was pressed
        for i in range(12):
            # Each pin is represented by a bit in the cap.touched() value. 1 means touched, 0 means untouched
            # pin_bit (a 1 shifted i positions to the left) indicates the position in the value for the i-th pin. 
            pin_bit = 1 << i

            # First check if transitioned from not touched to touched.
            if cap.touched() & pin_bit:
                print('{0} touched!'.format(i))
                cv2.imshow('SensShow',visuals[i])
                cv2.waitKey(1000)
		while cap.touched() & pin_bit:
		    print '{0} still touched...'.format(i) 
		    time.sleep(1)

    # wait a short period before repeating.
    time.sleep(0.1)
