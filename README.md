# pythonopenCV3
In this project we create code that uses the camera to capture hand signals
This code captures video from the camera, processes each frame to detect hand movements using color segmentation, and displays the video with the detected hand highlighted. You can adjust the color range for better skin detection based on your environment and lighting conditions.
DOCUMENTATION
import cv2: Imports the OpenCV library, which is used for image and video processing.
import numpy as np: Imports the NumPy library, which is used for numerical operations and handling arrays.
lower_skin: Defines the lower boundary for the HSV color values that will be considered as skin color.
upper_skin: Defines the upper boundary for the HSV color values that will be considered as skin color.
cap = cv2.VideoCapture(0): Opens the default camera (index 0) for capturing video.
while True: Starts an infinite loop to continuously capture frames from the camera.
ret, frame = cap.read(): Captures a single frame from the camera.
if not ret: break: Breaks the loop if the frame is not captured successfully.
frame = cv2.flip(frame, 1): Flips the frame horizontally to create a mirror image.
hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV): Converts the frame from BGR (default color space in OpenCV) to HSV color space. HSV stands for Hue, Saturation, and Value, which is more suitable for color-based segmentation.
mask = cv2.inRange(hsv, lower_skin, upper_skin): Creates a binary mask where pixels within the specified skin color range are white (255) and all other pixels are black (0).
mask = cv2.erode(mask, None, iterations=2): Erodes the mask to remove small white noise. Erosion reduces the size of the white regions.
mask = cv2.dilate(mask, None, iterations=2): Dilates the mask to restore the eroded parts and connect white regions. Dilation increases the size of the white regions.
mask = cv2.GaussianBlur(mask, (5, 5), 0): Applies a Gaussian blur to the mask to further reduce noise and smooth the edges.
contours, _ = cv2.findContours(mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE): Finds the contours in the binary mask. Contours are curves that join all the continuous points along a boundary with the same color or intensity.
if len(contours) > 0: Checks if at least one contour was found.
c = max(contours, key=cv2.contourArea): Finds the largest contour based on the contour area.
((x, y), radius) = cv2.minEnclosingCircle(c): Computes the minimum enclosing circle of the largest contour, which is a circle that completely encloses the contour.
M = cv2.moments(c): Calculates the moments of the largest contour. Moments are used to calculate the center of mass or centroid of the contour.
if M["m00"] > 0: Checks if the zeroth moment is greater than zero to avoid division by zero.
center = (int(M["m10"] / M["m00"]), int(M["m01"] / M["m00"])): Calculates the centroid of the contour
if radius > 10: Checks if the radius of the enclosing circle is greater than a minimum size to filter out small objects.
cv2.circle(frame, (int(x), int(y)), int(radius), (0, 255, 255), 2): Draws the enclosing circle on the frame.
cv2.circle(frame, center, 5, (0, 0, 255), -1): Draws a small circle at the centroid of the contour.
cv2.imshow('Frame', frame): Displays the frame with the detected hand movement in a window named 'Frame'.
if cv2.waitKey(1) & 0xFF == ord('q'): Waits for 1 millisecond for a key press and checks if the 'q' key was pressed.
break: Breaks the loop if the 'q' key is pressed.
cap.release(): Releases the camera resource.
cv2.destroyAllWindows(): Closes all OpenCV windows.
This code captures video from the camera, processes each frame to detect hand movements using color segmentation, and displays the video with the detected hand highlighted. You can adjust the color range for better skin detection based on your environment and lighting conditions.
