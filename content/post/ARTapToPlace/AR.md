+++
title = 'AR app - Tap To Place'
date = 2025-01-30
categories = ["AR","Unity"]
author = "jiwon"
draf = true
+++

In this project, **Tap to Place**, allows users to place **3D objects on real-world surfaces** using their phone’s camera. The app detects the floor, places objects, and enables users to **scale & move them dynamically**.

---

## 1. **Project Overview**  
**Technology Used:** **AR Foundation, Unity, C#**  
**Core Features:**  
- **Surface Detection**: Uses the phone camera to recognize real-world flat surfaces.  
- **Tap to Place**: Tap on a detected surface to place a 3D object.  
- **Scale & Move**: Pinch-to-zoom to resize and drag to reposition objects.  

---

## 2. **How It Works**  

### a. **Surface Detection**  
- The app uses **AR Plane Detection** to identify floors.  
- **AR Raycasting** helps determine where the 3D object should be placed.  

```csharp
void DetectSurface(Vector2 touchPosition)
{
    if (arRaycastManager.Raycast(touchPosition, hitResults, TrackableType.PlaneWithinPolygon))
    {
        Pose hitPose = hitResults[0].pose;
        PlaceObject(hitPose);
    }
}
```

### b. **Placing 3D Objects**  
- A **prefab** is instantiated at the detected position.  

```csharp
void PlaceObject(Pose spawnPosition)
{
    if (spawnedObject == null)
    {
        spawnedObject = Instantiate(objectPrefab, spawnPosition.position, spawnPosition.rotation);
    }
}
```

### c. **Scaling & Moving the Object**  
- **Pinch Gesture** to scale the object.  
- **Drag Gesture** to move it around the surface.  

```csharp
void ScaleObject(float scaleFactor)
{
    spawnedObject.transform.localScale *= scaleFactor;
}
```

---

## 3. **Setting Up AR Foundation in Unity**  

1️⃣ **Install AR Foundation**  
- Go to **Unity Package Manager** → Install **AR Foundation & ARCore XR Plugin (Android) / ARKit XR Plugin (iOS)**  

2️⃣ **Setup AR Session**  
- Add **AR Session** and **AR Session Origin** to the scene.  

3️⃣ **Enable Plane Detection**  
- Attach an **AR Plane Manager** component to detect surfaces.  

4️⃣ **Implement Raycasting**  
- Use **ARRaycastManager** to find valid placement points.  

---

## 4. **Future Improvements**  
🔹 Object rotation using touch gestures.  
🔹 Multi-object placement for more interactive scenes.  
🔹 Save and reload placed objects using persistent AR.  

---

## 5. Testing 

<div style="text-align: center;">
  <video width="600" height="600" controls>
    <source src="/videos/ARProject_JiwonKANG.mov" type="video/mp4">
  </video>
</div>
