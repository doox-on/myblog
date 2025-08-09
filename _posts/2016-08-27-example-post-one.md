---
title: Smart Autonomous Trash Can Robot
feature_image: "https://picsum.photos/2560/600?image=872"
---

## Smart Autonomous Trash Can Robot

An embedded systems project featuring autonomous line-following navigation, real-time trash level monitoring, and a touchless, automated lid.

{% include video.html id="uYw_aHjiI6A" %}

### Project Overview

This project is a smart, autonomous trash can robot designed to navigate predefined paths in an indoor environment. It automates trash collection by following a designated line, monitoring its internal capacity in real-time, and providing a hygienic, touchless lid mechanism. The entire system is managed by a Real-Time Operating System (RTOS) to ensure smooth and concurrent operation of all subsystems.

> **[Recommendation]**
> Insert a short video (30-60 seconds) here demonstrating the robot in action. Show it following the line, the lid opening for a user, and a close-up of the LCD screen displaying the fill level. This is the most effective way to showcase the final product.

### Key Features

* **Autonomous Line-Following Navigation**
    * Utilizes an array of analog line sensors to detect a marked path on the floor.
    * A motor controller interprets sensor data to dynamically adjust the speed and direction of three independent motors, ensuring the robot remains on its designated route without manual intervention.

* **Real-Time Trash Level Monitoring**
    * An upward-facing ultrasonic sensor, combined with a timer, precisely calculates the remaining capacity in the bin.
    * The fill-level percentage is displayed in real-time on an LCD screen, providing clear visual feedback and alerting users when the bin needs emptying.

* **Automated Touchless Lid**
    * A forward-facing ultrasonic sensor detects user motion near the top of the bin.
    * A servo motor is triggered upon motion detection to automatically open and close the lid, offering a convenient and hygienic hands-free experience.

> **[Recommendation]**
> Include a high-quality photo gallery here. Show close-ups of the key components: the line sensor array on the bottom, the ultrasonic sensors, the servo-lid mechanism, the motor/wheel assembly, and the main controller board.

### System Architecture & Concurrency

The system's stability and responsiveness are managed by a **Real-Time Operating System (RTOS)**. This allows for robust multitasking by dedicating separate threads for critical processes, including:
* Motor control and navigation adjustments.
* Sensor data polling and LCD screen updates.
* Lid mechanism operation.

To prevent race conditions and ensure safe operation, a **mutex** was implemented. For example, the mutex ensures that the robot's motors are safely halted before the lid-opening sequence is initiated, preventing conflicts between the navigation and user-interaction threads.

> **[Recommendation]**
> This is an ideal place for a system architecture or block diagram. A visual representation of how the microcontroller, sensors, motors, and RTOS threads interact would be very powerful for a technical audience.

### Challenges & Solutions

Throughout the development process, we addressed several key engineering challenges:

1.  **Challenge: Control Algorithm Selection**
    * We initially considered a fully analog PID controller for its potential robustness. However, its implementation complexity, tuning time, and high sensitivity to environmental factors (like the low contrast between the line and floor in our lab) made it impractical for our timeline.
    * **Solution:** We implemented a more discrete, simpler control algorithm. This pragmatic decision allowed for rapid development and effective tuning within our specific lab environment, achieving reliable line-following performance.

2.  **Challenge: Mechanical Stability and Power Management**
    * The initial two-wheeled design suffered from poor weight distribution, resulting in insufficient torque to move the robot consistently.
    * **Solution:** We re-engineered the chassis to include a third wheel for improved balance and traction. This required recalibrating motor speeds and subsequently increasing the battery capacity to meet the higher power demand, which in turn required further adjustments to optimize the new weight distribution. This iterative process was key to achieving a mechanically sound and functional robot.

### Comparison to Commercial Systems

This project integrates features commonly found in separate commercial products into a single, specialized device.
* It shares navigational principles with **autonomous vacuum robots** but is specialized to follow a predefined path rather than mapping open spaces.
* It incorporates the hygienic benefits of **motion-activated trash cans** but enhances this with mobility and capacity-sensing.

The unique value of this project lies in its synthesis of these functionalities for a targeted application like automated waste collection in offices or labs.

### Future Enhancements

With additional time and resources, the project could be improved with the following features:

* **Advanced Control Algorithm:** Transition to a fully tuned PID or similar analog controller for smoother and more responsive navigation.
* **Obstacle Detection & Avoidance:** Integrate additional sensors (e.g., LiDAR or IR) to detect and navigate around unexpected obstacles like people or objects.
* **Form Factor & Power Optimization:** Redesign the chassis for superior weight distribution and implement power-saving modes to extend battery life.
* **Automated "Return to Base" Functionality:** Program a routine for the robot to autonomously navigate to a designated "emptying station" once full.
* **Enhanced Sensor Robustness:** Add a frontal LED array to provide consistent lighting for the line sensors, removing dependencies on ambient light and improving navigational reliability.

### Technology Stack

* **Core:** RTOS, C/C++
* **Hardware:** Microcontroller, Ultrasonic Sensors, Analog Line Follower Sensors, Servo Motor, DC Motors, LCD Display
* **Chassis:** 3D Printed / Custom Fabricated Components
