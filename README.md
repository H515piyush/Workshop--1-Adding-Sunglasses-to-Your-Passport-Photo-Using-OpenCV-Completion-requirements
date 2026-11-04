# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

### NAME :Piyush kumar

### REG NO : 212223220075

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

Feel free to fork, contribute, or customize this project for your creative needs!

## Program :
```
# Import libraries
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Load the Face Image
faceImage = cv2.imread('pass.JPeG')
plt.imshow(faceImage[:,:,::-1]);plt.title("Face")

faceImage.shape
(700, 524, 3)

#resized_faceImage.shape
faceImage.shape
(700, 524, 3)

# Load the Sunglass image with Alpha channel
# (http://pluspng.com/sunglass-png-1104.html)
glassPNG = cv2.imread('glass.png',-1)
plt.imshow(glassPNG[:,:,::-1]);plt.title("glassPNG")

# Resize the image to fit over the eye region
glassPNG = cv2.resize(glassPNG,(190,50))
print("image Dimension ={}".format(glassPNG.shape))
image Dimension =(50, 190, 3)
# Separate the Color and alpha channels
glassBGR = glassPNG[:,:,0:3]
glassMask1 = cv2.cvtColor(glassPNG, cv2.COLOR_BGR2GRAY)
# Display the images for clarity
plt.figure(figsize=[15,15])
plt.subplot(121);plt.imshow(glassBGR[:,:,::-1]);plt.title('Sunglass Color channels');
plt.subplot(122);plt.imshow(glassMask1,cmap='gray');plt.title('Sunglass Alpha channel');

# Make a copy
#faceWithGlassesNaive = resized_faceImage.copy()
faceWithGlassesNaive = faceImage.copy()

# Replace the eye region with the sunglass image
faceWithGlassesNaive[190:240,150:340] = glassBGR
plt.imshow(faceWithGlassesNaive[...,::-1])

# Make the dimensions of the mask same as the input image.
# Since Face Image is a 3-channel image, we create a 3 channel image for the mask
glassMask = cv2.merge((glassMask1,glassMask1,glassMask1))

# Make the values [0,1] since we are using arithmetic operations
glassMask = np.uint8(glassMask/255)

# Make a copy
faceWithGlassesArithmetic = faceImage.copy()

# Get the eye region from the face image
eyeROI= faceWithGlassesArithmetic[135:185,110:300]

# Use the mask to create the masked eye region
maskedEye = cv2.multiply(eyeROI,(1-  glassMask ))

# Use the mask to create the masked sunglass region
maskedGlass = cv2.multiply(glassBGR,glassMask)

# Combine the Sunglass in the Eye Region to get the augmented image
eyeRoiFinal = cv2.add(maskedEye, maskedGlass)

# Display the intermediate results
plt.figure(figsize=[20,20])
plt.subplot(131);plt.imshow(maskedEye[...,::-1]);plt.title("Masked Eye Region")
plt.subplot(132);plt.imshow(maskedGlass[...,::-1]);plt.title("Masked Sunglass Region")
plt.subplot(133);plt.imshow(eyeRoiFinal[...,::-1]);plt.title("Augmented Eye and Sunglass")

# Make a fresh copy of the original image
faceWithGlassesArithmetic = faceImage.copy()

# Create a mask to remove the white background
glassGray = cv2.cvtColor(glassBGR, cv2.COLOR_BGR2GRAY)
_, glassMask = cv2.threshold(glassGray, 240, 255, cv2.THRESH_BINARY_INV)

# Get the inverse mask
glassMaskInv = cv2.bitwise_not(glassMask)

# Select the eye region
y1, y2 = 200, 250
x1, x2 = 200, 390

roi = faceWithGlassesArithmetic[y1:y2, x1:x2]

# Apply sunglasses without the white background
roi_bg = cv2.bitwise_and(roi, roi, mask=glassMaskInv)
glass_fg = cv2.bitwise_and(glassBGR, glassBGR, mask=glassMask)

# Combine both images
faceWithGlassesArithmetic[y1:y2, x1:x2] = cv2.add(roi_bg, glass_fg)

# Display the final result
plt.figure(figsize=[20,20])

plt.subplot(121)
plt.imshow(faceImage[:,:,::-1])
plt.title("Original Image")

plt.subplot(122)
plt.imshow(faceWithGlassesArithmetic[:,:,::-1])
plt.title("With Sunglasses")
```
# Output
<img width="334" height="434" alt="download" src="https://github.com/user-attachments/assets/555d3301-043e-4119-bdf9-f5b1e46e1aaa" />
<img width="1606" height="1006" alt="download" src="https://github.com/user-attachments/assets/1c1c1787-cbc0-4c70-ac04-e0c82e1bdbe3" />
<img width="1597" height="184" alt="download" src="https://github.com/user-attachments/assets/a34e65b3-f6a4-427a-85a5-a8c9d852ff2e" />
<img width="334" height="418" alt="download" src="https://github.com/user-attachments/assets/ca906f44-be26-44e3-8c05-0cb9ce93fde2" />
<img width="1210" height="203" alt="download" src="https://github.com/user-attachments/assets/aada212f-b6c0-4b89-b65d-11598d27a9c2" />
<img width="552" height="401" alt="download" src="https://github.com/user-attachments/assets/34685caf-2f80-46ad-b0fc-5ac861775dd6" />




