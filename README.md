# Object-Detection-based-on-color

## 💫 About Developer:

### Banish J

🤖 AI & ML Developer | Data Science & Analytics Expert  
🌐 Web Apps & IoT Skilled | Awesome UI Creator  
💻 AI is my main focus! 👾

## 📞 Contact
##### **☎️**   [9444333914](tel:9444333914)
##### **📧**  [mail@banish.in](mailto:mail@banish.in)
##### **🌐**  [Banish](https://www.banish.in)

## Core Concept

***This code captures video from the webcam, processes each frame to detect objects of a specified color (defined by HSV color ranges), highlights the detected objects, and displays the results in a window. The program prints directions based on the position of the detected object in the frame and can be terminated by pressing the 'q' key.***

## Importing Necessary Libraries
```python
import imutils
import cv2
```
- **imutils**: A convenience library for basic image processing functions, making it easier to work with OpenCV.
- **cv2**: The OpenCV library for computer vision tasks.

### Defining Color Ranges for Detection
```python
####for Phone in blue
##redLower = (101,101,90)         
##redUpper = (121,203,191)

####for a flower box
##redLower = (124,49,79)       
##redUpper = (179,255,255)

###for (green balm)
##redLower = (68,41,75)
##redUpper = (84,188,139)

###for face
##redLower = (0,23,38)
##redUpper = (61,214,146)

###for Hit
##redLower = (124,49,79)       
##redUpper = (179,255,255)

###for Green T-shirt
##redLower = (58,206,0)       
##redUpper = (100,255,255)

#for Light source
redLower = (0,0,255)       
redUpper = (0,0,255)
```
- These are predefined HSV color ranges for detecting various objects. Uncomment the desired range for different objects.
- Currently, the color range for a light source is being used.

### Initializing the Webcam
```python
camera = cv2.VideoCapture(0)
```
- **cv2.VideoCapture(0)**: Initializes the webcam for video capture. The argument `0` typically refers to the default webcam on the computer.

### Real-Time Object Detection Loop
```python
while True:
    (grabbed, frame) = camera.read()
    frame = imutils.resize(frame, width=1000)
    blurred = cv2.GaussianBlur(frame, (11, 11), 0)
    hsv = cv2.cvtColor(blurred, cv2.COLOR_BGR2HSV)
```
- **camera.read()**: Captures a frame from the webcam. `grabbed` indicates if the frame was read correctly, and `frame` stores the captured image.
- **imutils.resize(frame, width=1000)**: Resizes the frame to a width of 1000 pixels while maintaining the aspect ratio.
- **cv2.GaussianBlur(frame, (11, 11), 0)**: Applies a Gaussian blur to the image to reduce noise and smooth the image.
- **cv2.cvtColor(blurred, cv2.COLOR_BGR2HSV)**: Converts the blurred image from BGR color space to HSV color space.

### Creating a Mask and Finding Contours
```python
    mask = cv2.inRange(hsv, redLower, redUpper)
    mask = cv2.erode(mask, None, iterations=2)
    mask = cv2.dilate(mask, None, iterations=2)
    cnts = cv2.findContours(mask.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)[-2]
    center = None
```
- **cv2.inRange(hsv, redLower, redUpper)**: Creates a binary mask where the white pixels represent the colors within the specified range.
- **cv2.erode(mask, None, iterations=2)**: Erodes the mask to remove small white noise.
- **cv2.dilate(mask, None, iterations=2)**: Dilates the mask to restore the eroded parts of the object.
- **cv2.findContours(mask.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)[-2]**: Finds contours in the mask. The `[-2]` index is used to grab the correct contours list based on the version of OpenCV.
- **center = None**: Initializes the center variable.

### Detecting and Highlighting the Object
```python
    if len(cnts) > 0:
        c = max(cnts, key=cv2.contourArea)
        ((x, y), radius) = cv2.minEnclosingCircle(c)
        M = cv2.moments(c)
        center = (int(M["m10"] / M["m00"]), int(M["m10"] / M["m00"]))
        if radius > 10:
            cv2.circle(frame, (int(x), int(y)), int(radius), (0, 255, 255), -1)
            print(center, radius)
            if radius > 250:
                 print("stop")
            else:
                if(center[0] < 150):
                    print("Right")
                elif(center[0] > 450):
                    print("Left")
                elif(radius < 250):
                    print("Front")
                else:
                    print("Stop")
```
- **if len(cnts) > 0**: Checks if any contours are found.
- **c = max(cnts, key=cv2.contourArea)**: Selects the largest contour based on area.
- **((x, y), radius) = cv2.minEnclosingCircle(c)**: Finds the smallest circle that can enclose the contour.
- **M = cv2.moments(c)**: Calculates image moments, which help in finding the center of the contour.
- **center = (int(M["m10"] / M["m00"]), int(M["m10"] / M["m00"]))**: Computes the center of the contour.
- **if radius > 10**: Filters out small objects based on the radius.
  - **cv2.circle(frame, (int(x), int(y)), int(radius), (0, 255, 255), -1)**: Draws a circle around the detected object.
  - **print(center, radius)**: Prints the center coordinates and radius of the detected object.
  - **if radius > 250**: Prints "stop" if the object is too close.
  - **if(center[0] < 150)**: Prints "Right" if the object is on the left side.
  - **if(center[0] > 450)**: Prints "Left" if the object is on the right side.
  - **if(radius < 250)**: Prints "Front" if the object is within the range but not too close.

### Displaying the Result
```python
    cv2.imshow("Frame", frame)
    key = cv2.waitKey(1)
    if key == ord("q"):
        break
```
- **cv2.imshow("Frame", frame)**: Displays the frame with the detected object.
- **key = cv2.waitKey(1)**: Waits for 1 millisecond for a key press.
- **if key == ord("q")**: Checks if the 'q' key is pressed to break the loop and stop the program.

### Releasing Resources
```python
camera.release()
cv2.destroyAllWindows()
```
- **camera.release()**: Releases the webcam resource.
- **cv2.destroyAllWindows()**: Closes all OpenCV windows.
