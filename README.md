# Adding-Sunglasses-to-image
## Name: Iniya E
## Reg no: 212224230096

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

## PROGRAM AND OUTPUT:

```
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Load Haar cascade for face detection (make sure you have this file in your path)
face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + "haarcascade_frontalface_default.xml")

# Load images
faceImage = cv2.imread("/content/photo.png")
glassPNG = cv2.imread("/content/sunglasses.png", cv2.IMREAD_UNCHANGED)

gray = cv2.cvtColor(faceImage, cv2.COLOR_BGR2GRAY)
faces = face_cascade.detectMultiScale(gray, 1.3, 5)

for (x, y, w, h) in faces:
    # Resize sunglasses to face width
    new_w = w
    new_h = int(new_w * glassPNG.shape[0] / glassPNG.shape[1])
    glass_resized = cv2.resize(glassPNG, (new_w, new_h), interpolation=cv2.INTER_AREA)

    # Separate channels
    if glass_resized.shape[2] == 4:
        glass_rgb = glass_resized[:, :, :3]
        alpha = glass_resized[:, :, 3]
    else:
        glass_rgb = glass_resized
        gray_glass = cv2.cvtColor(glass_rgb, cv2.COLOR_BGR2GRAY)
        _, alpha = cv2.threshold(gray_glass, 240, 255, cv2.THRESH_BINARY_INV)

    # Masks
    mask = cv2.merge([alpha, alpha, alpha])
    mask_inv = cv2.bitwise_not(mask)

    # Position: slightly above middle of face rectangle
    y_offset = int(y + h * 0.3)   # about eye level
    x_offset = x

    # Ensure ROI fits inside
    y1, y2 = y_offset, min(faceImage.shape[0], y_offset + new_h)
    x1, x2 = x_offset, min(faceImage.shape[1], x_offset + new_w)

    roi = faceImage[y1:y2, x1:x2]
    mask_roi = mask[0:(y2 - y1), 0:(x2 - x1)]
    mask_inv_roi = mask_inv[0:(y2 - y1), 0:(x2 - x1)]
    glass_roi = glass_rgb[0:(y2 - y1), 0:(x2 - x1)]

    # Blend
    bg = cv2.bitwise_and(roi, roi, mask=cv2.cvtColor(mask_inv_roi, cv2.COLOR_BGR2GRAY))
    fg = cv2.bitwise_and(glass_roi, glass_roi, mask=cv2.cvtColor(mask_roi, cv2.COLOR_BGR2GRAY))
    combined = cv2.add(bg, fg)

    faceImage[y1:y2, x1:x2] = combined

# Show results
plt.figure(figsize=(15,5))

plt.subplot(1,3,1)
plt.imshow(cv2.cvtColor(cv2.imread("/content/photo.png"), cv2.COLOR_BGR2RGB))
plt.title("Original Face")
plt.axis("off")

plt.subplot(1,3,2)
plt.imshow(cv2.cvtColor(glassPNG[:,:,:3], cv2.COLOR_BGR2RGB))
plt.title("Sunglasses")
plt.axis("off")

plt.subplot(1,3,3)
plt.imshow(cv2.cvtColor(faceImage, cv2.COLOR_BGR2RGB))
plt.title("Face with Sunglasses (Fitted)")
plt.axis("off")

plt.show()
```
## ORIGINAL PHOTO:
<img width="673" height="653" alt="image" src="https://github.com/user-attachments/assets/9fa27776-eaaf-4579-86cd-3e7dc07f16b5" />

## SUNGLASS:
<img width="883" height="486" alt="image" src="https://github.com/user-attachments/assets/2089a017-3150-48c4-9d7f-c303375f3454" />


## WITH SUNGLASS:
<img width="818" height="677" alt="image" src="https://github.com/user-attachments/assets/73504a9e-7e90-4c32-b3ad-e111eb7e8894" />



<img width="1607" height="242" alt="image" src="https://github.com/user-attachments/assets/35124854-7b76-4a75-b2d1-44d3b6a85cc8" />


<img width="1637" height="757" alt="image" src="https://github.com/user-attachments/assets/9bca6d4c-6d7d-406a-b57f-5c4707e66ea1" />
