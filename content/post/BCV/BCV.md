+++
title = 'Final Project - Basic Computer Vision'
date = 2024-11-28
categories = ["BCV","OpenCV"]
author = "jiwon"
draf = true
+++


# Project Goals

### Marker based detection
- Extract keypoints and descriptors from a marker image.
- Match the marker with live webcam footage in real time.
- Enhance robustness of matching.
- Overlay marker image on detected locations
- Compare performance


```python
#Import the necessary libraries
import cv2
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import os
import time
```

## 2. Create a marker image


```python
#read the image and change to grayscale for detect the marker 
image = cv2.imread('chariot.jpg', cv2.COLOR_BGR2GRAY)

#show the original image
plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))  
plt.title("Original Image")
plt.axis("off")  
plt.show()
```

<div style="text-align: center;">
  <img src="/images/output_4_0.png"  width="25%">
  <p style="font-size: 12px; color: gray;"></p>
</div>


```python
# extract the keypoints and descriptors from image
sift = cv2.SIFT_create()
keypoints_marker, descriptors_marker = sift.detectAndCompute(image, None)

#save the marker imgae with detected keypoints visualized
output_image = cv2.drawKeypoints(
    image,
    keypoints_marker, 
    None, 
    flags=cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS
)

#save the marker image
cv2.imwrite('marker_image_chariot_sift.jpg', output_image) 

#visualize the marker image
plt.imshow(cv2.cvtColor(output_image, cv2.COLOR_BGR2RGB)) 
plt.title("Marker Image with Keypoints_SIFT")
plt.axis("off")
plt.show()
```

<div style="text-align: center;">
  <img src="/images/output_5_0.png"  width="25%">
  <p style="font-size: 12px; color: gray;"></p>
</div>
 
```python
# extract the keypoints and descriptors from image
orb = cv2.ORB_create()
keypoints_marker, descriptors_marker = orb.detectAndCompute(image, None)

output_image = cv2.drawKeypoints(
    image,
    keypoints_marker, 
    None, 
    flags=cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS
)

#save the marker image
cv2.imwrite('marker_image_chariot_orb.jpg', output_image) 

#visualize the marker image
plt.imshow(cv2.cvtColor(output_image, cv2.COLOR_BGR2RGB)) 
plt.title("Marker Image with Keypoints_ORB")
plt.axis("off")
plt.show()
```
<div style="text-align: center;">
  <img src="/images/output_6_0.png"  width="25%">
  <p style="font-size: 12px; color: gray;"></p>
</div>


- As we can see above, due to the difference in keypoint detection methods, the keypoints are detected differently in the marker image.

- **ORB** quickly finds corners based on the FAST algorithm. Since it is designed for real-time processing, it detects a limited number of keypoints to maintain speed.

- **SIFT** finds keypoints in various sizes and scales in the image. It finds strong features in all areas of the image using the **DoG method**. Keypoint detection is not limited, and all possible keypoints are explored.


- Considering these differences, I will choose SIFT for its ability to guarantee high matching accuracy in various environments and proceed with real-time matching.

## 2. Matching Descriptor

- **Matching** is a problem of finding pairs corresponding to the same location of the same object when a set of `dc1` extracted from an image and a set of `dc2` extracted from a video shown by a real-time webcam are given. 

- Since the marker image has an object alone, a relatively reliable descriptor is detected, and since the video contains multiple objects, there are many descriptors and a lot of noise.

- **Brute-Force Matching (BFMatcher)** is a method that compares all possible matches between two sets of descriptors.

- I use BFMatcher with the knn matching strategy. The algorithm identifies the descriptor `dc2j` in the real-time video that has the shortest distance to the descriptor `dc1i` from the marker image. If the condition  
  \[
  d(dc1i, dc2j) < T
  \]
  is satisfied, the pair is considered a match.



```python
def initialize_marker(marker_path):
    marker_image = cv2.imread(marker_path, cv2.IMREAD_COLOR)
    gray_marker_image = cv2.cvtColor(marker_image, cv2.COLOR_BGR2GRAY)
  
    sift = cv2.SIFT_create()
    keypoints_marker, descriptors_marker = sift.detectAndCompute(gray_marker_image, None)
    
    print(f"Initialized marker image '{marker_path}'.")
    return marker_image, gray_marker_image, keypoints_marker, descriptors_marker

marker_path = 'marker_image_chariot.jpg'
marker_image, gray_marker_image, keypoints_marker, descriptors_marker = initialize_marker(marker_path)

#make a directory for saving results
output_dir = "captures"
os.makedirs(output_dir, exist_ok=True)
frame_count = 0
```


### Real-time matching


```python
#start webcam
cap = cv2.VideoCapture(0)

#initialize Brute-Force matcher
bf = cv2.BFMatcher()

while True:
    #capture a frame from the webcam
    ret, frame = cap.read()
    if not ret:
        break
    
    gray_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    #detect keypoints and descriptors in the current frame 
    keypoints_frame, descriptors_frame = sift.detectAndCompute(gray_frame, None)

    # keypoint matching - knn matching strategy
    matches = bf.knnMatch(descriptors_marker, descriptors_frame, k=2)

    good_matches = []
    for m, n in matches:
        if m.distance < 0.75 * n.distance:
            good_matches.append(m)

    #visualize
    frame_with_matches = cv2.drawMatches(
        marker_image,
        keypoints_marker,
        frame, 
        keypoints_frame, 
        good_matches, None,
        flags=cv2.DrawMatchesFlags_NOT_DRAW_SINGLE_POINTS
    )

    cv2.imshow('Matches', frame_with_matches)

    key = cv2.waitKey(1) & 0xFF
    if key == 27:  
        print("ESC pressed. Exiting.")
        break
    elif key == ord('c'):  #capture the img by pressing 'c'
        frame_count += 1
        capture_path = os.path.join(output_dir, f"capture_{frame_count}.jpg")
        cv2.imwrite(capture_path, frame_with_matches)
        print(f"Captured and saved: {capture_path}")

cap.release()
cv2.destroyAllWindows()

```

<div style="text-align: center;">
  <img src="/images/capture_3.jpg"  width="50%">
  <p style="font-size: 12px; color: gray;"></p>
</div>


- We can see the matching result between the marker image and real-time video. 

- However, some incorrect matches can also be observed. To address this, the `findHomography()` function is used, which calculates the transformation matrix using the indices of selected descriptor matching pairs. This process is refined using the RANSAC algorithm.

- Then, add a rectangle is drawn on the video to represent the transformation, aligning with the four corner points of the marker image.

### Compute Homography


```python
cap = cv2.VideoCapture(0)

bf = cv2.BFMatcher()

while True:
    ret, frame = cap.read()
    if not ret:
        print("Failed to capture frame from webcam. Exiting.")
        break

    gray_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    keypoints_frame, descriptors_frame = sift.detectAndCompute(gray_frame, None)

    matches = bf.knnMatch(descriptors_marker, descriptors_frame, k=2)

    good_matches = []
    for m, n in matches:
        if m.distance < 0.75 * n.distance:
            good_matches.append(m)

    #compute Homography 
    if len(good_matches) > 10: 
        src_pts = np.float32([keypoints_marker[m.queryIdx].pt for m in good_matches]).reshape(-1, 1, 2)
        dst_pts = np.float32([keypoints_frame[m.trainIdx].pt for m in good_matches]).reshape(-1, 1, 2)

        H, mask = cv2.findHomography(src_pts, dst_pts, cv2.RANSAC, 5.0)

        if H is not None:
            h_marker, w_marker = gray_marker_image.shape
            corners_marker = np.float32([
                [0, 0],
                [w_marker, 0],
                [w_marker, h_marker],
                [0, h_marker]
            ]).reshape(-1, 1, 2)

            transformed_corners = cv2.perspectiveTransform(corners_marker, H)
            frame = cv2.polylines(frame, [np.int32(transformed_corners)], isClosed=True, color=(0, 255, 0), thickness=3)

    else:
        print("Not enough matches to calculate Homography.")

    match_visual = cv2.drawMatches(
        marker_image, keypoints_marker, 
        frame, keypoints_frame, 
        good_matches, None, 
        flags=cv2.DrawMatchesFlags_NOT_DRAW_SINGLE_POINTS
    )
    cv2.imshow('Matching Results', match_visual)

    
    if cv2.waitKey(1) & 0xFF == 27: 
        print("ESC pressed. Exiting.")
        break

cap.release()
cv2.destroyAllWindows()

```


<div style="text-align: center;">
  <img src="/images/homography.jpg"  width="50%">
  <p style="font-size: 12px; color: gray;"></p>
</div>


### Marker Overlay


```python
cap = cv2.VideoCapture(0)

bf = cv2.BFMatcher()

while True:
    ret, frame = cap.read()
    if not ret:
        print("Failed to capture frame from webcam. Exiting.")
        break

    gray_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    keypoints_frame, descriptors_frame = sift.detectAndCompute(gray_frame, None)

    matches = bf.knnMatch(descriptors_marker, descriptors_frame, k=2)

    good_matches = [m for m, n in matches if m.distance < 0.75 * n.distance]

    if len(good_matches) > 10:  
        src_pts = np.float32([keypoints_marker[m.queryIdx].pt for m in good_matches]).reshape(-1, 1, 2)
        dst_pts = np.float32([keypoints_frame[m.trainIdx].pt for m in good_matches]).reshape(-1, 1, 2)

        H, mask = cv2.findHomography(src_pts, dst_pts, cv2.RANSAC, 5.0)

        if H is not None:
            h_marker, w_marker, _ = marker_image.shape
            warped_marker = cv2.warpPerspective(marker_image, H, (frame.shape[1], frame.shape[0]))

            #create the overlay mask
            marker_mask = cv2.warpPerspective(
                np.ones((h_marker, w_marker, 3), dtype=np.uint8),
                H,
                (frame.shape[1], frame.shape[0])
            ).astype(bool)

            frame[marker_mask] = warped_marker[marker_mask]

        else:
            print("Homography calculation failed.")
    else:
        print("Not enough matches to calculate Homography.")


    cv2.imshow('Marker Overlay', frame)

    if cv2.waitKey(1) & 0xFF == 27:  
        print("ESC pressed. Exiting.")
        break

cap.release()
cv2.destroyAllWindows()

```
<div style="text-align: center;">
  <img src="/images/overlay_marker_image.jpg"  width="50%">
  <p style="font-size: 12px; color: gray;"></p>
</div>


## Compare performance : ORB vs SIFT


```python
marker_image = cv2.imread('chariot.jpg', cv2.IMREAD_COLOR)
gray_marker_image = cv2.cvtColor(marker_image, cv2.COLOR_BGR2GRAY)

orb = cv2.ORB_create()
sift = cv2.SIFT_create()

keypoints_orb, descriptors_orb = orb.detectAndCompute(gray_marker_image, None)
keypoints_sift, descriptors_sift = sift.detectAndCompute(gray_marker_image, None)

cap = cv2.VideoCapture(0)


bf = cv2.BFMatcher()

print("Press 's' to start matching. Press 'ESC' to quit.")

#wait until the real-time video prepared
while True:
    ret, frame = cap.read()
    if not ret:
        print("Failed to capture frame from webcam. Exiting.")
        break

    cv2.imshow("Press 's' to start", frame)

    key = cv2.waitKey(1) & 0xFF
    if key == ord('s'):  # start by pressing 's'
        print("Starting real-time matching...")
        break
    elif key == 27: 
        print("ESC pressed. Exiting.")
        cap.release()
        cv2.destroyAllWindows()
        exit()

performance_data = []
while True:
    ret, frame = cap.read()
    if not ret:
        print("Failed to capture frame from webcam. Exiting.")
        break

    gray_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    for algo_name, detector, descriptors_marker in [("ORB", orb, descriptors_orb), ("SIFT", sift, descriptors_sift)]:
        start_time = time.time()

        keypoints_frame, descriptors_frame = detector.detectAndCompute(gray_frame, None)

        if descriptors_frame is None or descriptors_marker is None:
            good_matches_count = 0
        else:
            matches = bf.knnMatch(descriptors_marker, descriptors_frame, k=2)
            good_matches = [m for m, n in matches if m.distance < 0.75 * n.distance]
            good_matches_count = len(good_matches)

        elapsed_time = time.time() - start_time

        performance_data.append({
            "Algorithm": algo_name,
            "Matches": good_matches_count,
            "Processing Time (s)": elapsed_time
        })

        match_visual = cv2.drawMatches(
            marker_image, keypoints_orb if algo_name == "ORB" else keypoints_sift, 
            frame, keypoints_frame, 
            good_matches, None, 
            flags=cv2.DrawMatchesFlags_NOT_DRAW_SINGLE_POINTS
        )
        cv2.imshow(f'Matches ({algo_name})', match_visual)

    if cv2.waitKey(1) & 0xFF == 27:
        print("ESC pressed. Exiting.")
        break

cap.release()
cv2.destroyAllWindows()

df = pd.DataFrame(performance_data)
print(df.groupby("Algorithm").mean())

```

                   Matches  Processing Time (s)
    Algorithm                                  
    ORB           3.461538             0.016121
    SIFT       1110.538462             0.886560


#### Result

- ORB
    - On average, only about 3 matches were successfully performed.
    - ORB is fast, but its matching performance is likely to be poor.

- SIFT
    - On average, about 1110 matches were successfully performed.
    - SIFT provides strong matching performance and shows high reliability.


- ORB is fast but has low matching performance. However, in real-time situations, the processing speed is fast. On the other hand, SIFT provides high matching performance and reliability, but the processing speed is slow. In real-time detection situations, the processing speed can be felt to be slow.

## Conclusion

1. Successfully implemented real-time matching using SIFT and `findHomography`.
2. Verified matching accuracy by visualizing keypoints, calculating Homography for transformation, and overlaying the marker image onto the detected region.
3. Integrated a robust filtering mechanism to ensure only good matches are used for accurate detection.

Through this project, I gained practical experience in utilizing OpenCV for real-time image matching. I also deepened my understanding of detectors and descriptors, such as `SIFT` and `ORB`, and how they impact the accuracy and efficiency of image matching.

