+++
title = 'Kinect'
date = 2024-10-10
categories = ["HCI"]
tags = ["HCI-Lab"]
author = "jiwon"
image = "/images/kinectimage.png"
draft = false
+++

## What is Kinect?

<div style="text-align: center;">
  <img src="/images/kinect.png" alt="Kinect" width="50%">
</div>

Kinect is a motion-sensing device used for recognizing human movement. Kinect includes a 3D depth camera, RGB camera, and microphone array, making it highly effective for tracking body positions and movements. Today, Kinect is widely used in research fields and various HCI projects beyond gaming.

## Purpose of a Virtual Environment

Using a virtual environment helps in managing project-specific dependencies independently. Each project can have its own set of libraries and versions, preventing conflicts with other projects and streamlining workflow.

### To create and activate a virtual environment

1. **Create the virtual environment folder**
- In the project directory, such as `D:\Users\Student\Desktop\kinect`, create a virtual environment using a hidden folder
   ```bash
   python -m venv .bonjour
   ```

2. **Activate the virtual environment**
- Use this command to activate it
   ```bash
   .\.bonjour\Scripts\activate
   ```

3. **Check if activated**
- If activated successfully, the prompt will display `(student)` before the directory path, indicating that the virtual environment is in use.

## Kinect and Sensor Configuration

Kinect has multiple sensors that capture both image and depth data simultaneously. Data is stored in the project’s `data` folder, with image and depth data organized separately.

## Examples of Kinect Functionalities

1. **Kinect Fusion Head Scanning**

<div style="text-align: center;">
  <img src="/images/headscanning.png" alt="Kinect" width="50%">
</div>

   - As shown in the image above, Kinect Fusion allows 3D head scanning, which reconstructs a detailed 3D model of the user's head. Users can adjust parameters like "Volume Max Integration Weight" and "Volume Voxel Resolution" to control the detail and quality of the scan.
   - This functionality is useful for applications that require 3D head models, which can be exported as `.STL`, `.OBJ`, or `.PLY` files for use in other software.

<div style="text-align: center;">
  <img src="/images/unity3dhead.png" alt="Kinect" width="50%">
</div>


2. **Depth Sensing and Mapping**

<div style="text-align: center;">
  <img src="/images/depthsensing.png" alt="Kinect" width="50%">
</div>

   - This image illustrates Kinect’s depth sensing capabilities, where objects are visualized based on their distance from the sensor. Black areas represent regions farthest from the Kinect, while closer areas appear in lighter shades.
   - This depth map allows for spatial awareness, essential for applications like gesture recognition and object tracking.


3. **Face Tracking**

<div style="text-align: center;">
  <img src="/images/facetraking.png" alt="Kinect" width="50%">
</div>

   - This image showcs Kinect’s face tracking functionality. Here, Kinect detects the user’s face and maps key facial features using a network of lines, making it ideal for applications requiring facial recognition, facial expression analysis, or real-time face-driven animations.

## Real-Time Data Collection and Processing

To initiate real-time data collection, run the `real_time.py` script:
```bash
python real_time.py
```
This script enables real-time detection of faces and bodies. It can be modified to add functionalities such as drawing bounding boxes around detected faces or adding other interactive elements.

## Kinect SDK and Toolkit

In our project, we’re using the Kinect SDK and the Kinect Developer Toolkit. These tools give us access to Kinect’s main features, like motion tracking, depth sensing, and 3D scanning, making it easier to work with the Kinect sensor. The SDK includes useful sample projects, such as **Skeleton Basics** for tracking body movements.


## Another example of Kinect - Skeleton Basics

<div style="text-align: center;">
  <video width="600" controls>
    <source src="/videos/skeleton.mp4" type="video/mp4">
  </video>
</div>
