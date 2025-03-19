+++
title = 'MRTK Project - Trolley Race Adventure'
date = 2025-01-30
categories = ["MRTK","Unity"]
author = "jiwon"
draf = true
+++

**Authors:** Jiwon Kang & Sara Naranjo  
**Project Type:** HoloLens 2 AR Game  

## 1. Game Overview  
Trolley Race Adventure is an immersive HoloLens 2 game where players race trolleys on dynamic tracks. The goal is to **collect good items (cherries 🍒) to earn points** while **avoiding bad items (bananas 🍌) that slow you down** and reach the finish line as quickly as possible.  

### 2. Controls  
- **Steering:** Press buttons to turn left/right  
- **Speed Control:** Use keyboard arrow keys (↑/↓) to adjust speed  

---

## 3. Game Assets  
**Source:** Unity Asset Store  

- **Trolleys (Playable Characters)**  
- **Circuit (Gameplay Levels)**  
- **Items (Cherries 🍒 / Bananas 🍌)**  

### 4. Vehicle Selection  
- Players choose a trolley before starting the game using a **slider**  
- The selected trolley spawns when the game starts  

### 5. Level Design  
- **Level 1 (Easy):** Fewer collectible and avoidable objects  
- **Level 2 (Hard):** More objects on the track  

---

## 6. How to Play  
1. **Select a vehicle → Choose a level → Click the Start button**  
2. **Control movement:** Use buttons to turn left/right  
3. **Adjust speed:** Use the keyboard arrow keys  
4. **Collect cherries (+points), avoid bananas (-points)**  
5. **Finish the track and view the scoreboard**  

---

## 7. Scoring System  
- **Good Items (Cherries 🍒):** Increase points  
- **Bad Items (Bananas 🍌):** Decrease points  
- **TextMeshPro UI** updates scores dynamically  
- The final score appears after completing the level  

---

## 8. MRTK3 Components Used  
| Feature | MRTK3 Component |
|---------|----------------|
| Vehicle Selection | Slider |
| Level Selection | Dropdown |
| Turning & Start Game | Button |
| In-game Instructions | Dialog |
| UI Scaling | Bounds Control |
| Game Settings | Near Menu |

---

## 9. Conclusion & Future Improvements  
**Achievements:**  
- Successfully integrated AR gameplay with MRTK3  
- Designed an interactive UI for a better player experience  

**Future Enhancements:**  
- Implement voice commands for controls  
- Introduce more complex level designs  
- Add obstacle-based challenges (mud zones, random objects)  

---

## 10. Testing

<div style="text-align: center;">
  <video width="600" controls>
    <source src="/videos/MRTK.mp4" type="video/mp4">
  </video>
</div>