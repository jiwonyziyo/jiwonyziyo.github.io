+++
title = "Lecture 1 - Affordances"
date = 2024-09-19
categories =["HCI"]
author = "jiwon"
image = "images/affordance.jpg"
draft = false
+++

Affordance gives users a visual hint on what actions they can take.

For example, when we see a button, we instinctively want to press it, or when we see a switch, we feel like pulling it. On an app or website, a rectangular box with a border makes us think we can click and type into it. These cues play into human psychology.

Thus, affordance is a crucial element of visual user interfaces. The clearer the visual cues are, the less ambiguity there is for the user to understand what action is expected.

In mobile environments, the importance of affordance becomes even more pronounced, as the small screen size and limited space make it harder for affordance to be as visually obvious. Therefore, clear affordance is critical to ensure smooth user interaction on mobile devices.

## Good case - Floor Traffic Lights
![trafficlight](images/floorsign.jpg)

**Clear Functional Communication** 
- Just like a door handle that instantly communicates its function, floor traffic lights, placed at pedestrian crossings, clearly indicate when a person should stop or walk. The placement on the ground provides a solution specifically for people who often look down at their phones while walking. Without any instructions, pedestrians can understand what action to take based on the light signals.

**Intuitive Usage Encouragement** 
- Pedestrians who walk while looking at their smartphones also unconsciously recognize the signals on the ground. While existing traffic lights are located above the head and are difficult to see unless you look up, floor traffic lights are designed to align with modern pedestrian behavior, particularly smartphone use. By positioning the signal at eye level for those looking down, the design naturally guides users to stop or walk.


## Bad case - Keyboard
![Keyboard](images/keyboard.jpg)

The placement of a power button above the delete key on a keyboard is a bad example of affordance. 

**Risk of Accidental Use** 
- When the power button is located too close to the delete key, users are more likely to press it accidentally, potentially shutting down the system unintentionally. The functions are too distinct to be placed so closely.

**Functional Mismatch**
- The delete key is frequently used, whereas the power button is not. Placing such an important function near a less frequently used key increases the risk of errors and reduces the overall usability.

**Contrary to User Expectations** 
- Users do not expect two very different functions to be positioned so closely together. This placement disrupts the intuitive understanding of how the keyboard should work.

### *How can we fix the bad case* ###

**Relocate the Power Button**
- The most straightforward solution is to move the power button to a location further away from frequently used keys like the delete key. For example, the power button could be placed at the right-top corner of the keyboard, near the function keys or in a separate, more isolated area where it’s less likely to be pressed by accident.

**Add a Confirmation Step**
- Implementing a confirmation step when the power button is pressed would prevent accidental shutdowns. For instance, instead of instantly shutting down the system, the button could trigger a prompt asking the user to confirm the action.
