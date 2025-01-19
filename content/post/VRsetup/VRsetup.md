+++
title = 'VR Development - Roll A Ball'
date = 2025-01-19
categories = ["HCI"]
tags = ["HCI-Lab"]
author = "jiwon"
draf = true
+++

### Implementing the Roll-a-Ball Game in VR

In this blog post, we will apply the **Roll-a-Ball** game to a VR environment. 

---

### 1. Initial Setup

#### Adjusting the Playboard
In a VR environment, the playboard will continuously fall. To prevent this:
- Add a **Plane** to provide a stable base.
- Place the playboard on a **Table** to ensure it remains at a reachable height.

<div style="text-align: center;">
  <img src="/images/14.png" width="50%">
  <p style="font-size: 12px; color: gray;">Initial Setup</p>
</div>

---

### 2. Installing Required Plugins

To develop for VR, we need to install the following Unity plugins:

#### OpenXR Plugin Installation
1. Go to **Window** > **Package Manager**.
2. Change **Packages** to "Unity Registry."
3. Search for **OpenXR Plugin** and install it.
   - Unity will restart automatically after installation.
4. After installation, navigate to **Edit** > **Project Settings** > **XR Plug-in Management** > **OpenXR**.
   - Ensure that the **OpenXR feature group** is enabled.
   - Set the appropriate interaction profile, such as **Oculus Touch Controller Profile**.

<div style="text-align: center;">
  <img src="/images/2.png" width="50%">
  <p style="font-size: 12px; color: gray;">OpenXR Plugin</p>
</div>

#### XR Interaction Toolkit Installation
1. Follow the same steps as above to install **XR Interaction Toolkit.**
2. Download the **Starter Assets** package.

<div style="text-align: center;">
  <img src="/images/1.png" width="50%">
  <p style="font-size: 12px; color: gray;">XR Interaction toolkit</p>
</div>

---

### 3. Project Settings

1. Navigate to **Edit** > **Project Settings** > **XR Plug-in Management.**
2. Check the **Oculus** option.
   - If a warning icon (!) appears, click **Fix All** to resolve it.

<div style="text-align: center;">
  <img src="/images/3.png"  width="50%">
  <p style="font-size: 12px; color: gray;">Project Setting</p>
</div>

---

### 4. Setting Up XR Interaction Toolkit

#### Applying Starter Assets
1. Go to **Assets** > **Samples** > **XR Interaction Toolkit** > **2.2.0** > **Starter Assets.**
2. Remove all default presets from **ActionBasedContinuousMoveProvider.default.**

<div style="text-align: center;">
  <img src="/images/4.png"  width="50%">
  <p style="font-size: 12px; color: gray;">Starter Assets</p>
</div>

<div style="display: flex; justify-content: center; align-items: center;">
  <div style="margin-right: 10px; text-align: center;">
    <img src="/images/5.png" width="100%">
    <p style="font-size: 12px; color: gray;"></p>
  </div>
  <div style="text-align: center;">
    <img src="/images/6.png"  width="100%">
    <p style="font-size: 12px; color: gray;"></p>
  </div>
</div>

---

### 5. Adding and Configuring XR Origin

#### Configuring XR Origin
1. Add **XR Origin (Action Based)** to the Hierarchy.
   - Ensure you select the one labeled *(Action Based)* to include the camera and controllers for both hands.
2. Delete the existing **Main Camera** from the Hierarchy.
3. Add the following components to **XR Origin:**
   - **Character Controller**
   - **Character Controller Driver**
4. Set **Tracking Origin Mode** to **Floor** in the XR Origin component.

<div style="text-align: center;">
  <img src="/images/7.png"  width="50%">
  <p style="font-size: 12px; color: gray;">XR Origin</p>
</div>

---

### 6. Setting Up the Locomotion System

#### Adding Locomotion System
1. Add **Locomotion System (Action Based)** from **XR > Locomotion System (Action Based)** in the Hierarchy.
2. Drag it into **XR Origin's Character Controller Driver** as the **Locomotion Provider.** (check the image of **XR Origin**)
3. In the Inspector panel, disable or remove:
   - **Teleportation Provider**
   - **Snap Turn Provider**


#### Configuring Movement
1. Add **Continuous Move Provider** and **Continuous Turn Provider.**
2. Disable **Use Reference** for:
   - **Right Hand Move Action** in Continuous Move Provider.
   - **Left Hand Turn Action** in Continuous Turn Provider.

<div style="text-align: center;">
  <img src="/images/8.png"  width="50%">
  <p style="font-size: 12px; color: gray;">Locomotion System</p>
</div>

---

### 7. Creating the Hand Controller

#### Hand Controller Prefab
1. Create an **Empty GameObject** and rename it to **HandController.**
2. Add a **Cube** and a **Cylinder** as children:
   - **Cube (Handle)**
     - Position: (0, 0, -0.11)
     - Scale: (0.04, 0.04, 0.1)
   - **Cylinder (Pad)**
     - Position: (0, 0, -0.05)
     - Scale: (0.07, 0.024, 0.07)
3. Remove the colliders from both objects.
4. In the **Mesh Renderer**, set **Cast Shadows** to **Off.**

<div style="text-align: center;">
  <img src="/images/10.png"  width="50%">
  <p style="font-size: 12px; color: gray;">Hand Controller</p>
</div>

#### Configuring the Right Hand Controller
1. In the Inspector, remove or disable the following components:
   - XR Ray Interactor
   - Line Renderer
   - XR Interactor Line Visual
2. Add an empty GameObject named **ModelParent** as a child of **RightHand Controller** and assign it to the XR Controller's **Model** field.

<div style="text-align: center;">
  <img src="/images/11.png"  width="50%">
  <p style="font-size: 12px; color: gray;">Right Hand Controller</p>
</div>

3. Since we need to grab the playboard, we need to add a collider.
    - In Add component, add a **Sphere Collider**, check **isTrigger**, and set the radius to 0.5.
    - Also add **XR direct interaction**.

<div style="text-align: center;">
  <img src="/images/12.png"  width="50%">
  <p style="font-size: 12px; color: gray;">Right Hand Controller Inspector</p>
</div>

---

### 8. Adding Interaction to the Playboard

1. Select the **Playboard** and add the **XR Grab Interactable** component.
2. Create an empty GameObject named **AttachPointL** under the Playboard.
3. Assign **AttachPointL** to the **Attach Transform** field of the XR Grab Interactable component.

<div style="text-align: center;">
  <img src="/images/13.png"  width="50%">
  <p style="font-size: 12px; color: gray;">Playboard</p>
</div>

---

### 9. Testing and Final Results

Once all configurations are complete, connect your **Oculus** headset to the computer and run the game to ensure everything works as expected. Below is the final result of our VR development:

<div style="text-align: center;">
  <video width="600" controls>
    <source src="/videos/vrvideo.mp4" type="video/mp4">
  </video>
</div>

---

By following these steps, you can successfully adapt your Roll-a-Ball game to VR !! 

