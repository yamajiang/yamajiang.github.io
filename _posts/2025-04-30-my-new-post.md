---
title: 'NavX'
date: 2025-04-30
permalink: /posts/2025/04/navx/
tags:
  - robotics
  - arduino
  - object detection
  - YOLOv8
  
---

NavX is a small-scale autonomous robot developed as part of a project for the EGN4060C Introduction to Robotics course at the University of Central Florida. Designed to navigate its environment independently, NavX is equipped with obstacle avoidance and object identification capabilities, allowing it to detect and respond to its surroundings in real-time. The robot is powered by a combination of technologies, including Arduino, Python, and the YOLOv8 object detection model. 

------

Abstract
======
Autonomous robotic systems have seen rapid advancements in recent years in many different industries. This follows the applications of self-driving vehicles, automation in workplaces, and intelligent navigation systems. In this project, we built and programed an autonomous object avoidance robot capable of avoiding objects in real time while identifying and reporting the detected objects to the user. NavX uses a combination of different sensors and embedded systems along with decision making algorithms and classification programs to incorporate a broader application of autonomous navigation applicable for different utilizations. Through the development of this project, our team was able to develop hands-on experience on key areas of robotics, machine learning, and decision making exploring the growing field of autonomous systems.  

![NavX](/images/navx.png)


Building NavX
======
My friend and I collaborated to build NavX for our class. As we lacked a hardware background, we used a robot kit that included essentials like an Arduino Uno, motors, sensors, an IR remote, and an ESP32 camera module. Assembling the robot helped us understand how each component contributed to making NavX autonomous.

I focused on the obstacle avoidance functionality, which I programmed using the Arduino IDE. We first mapped out each IR remote button's hex code, enabling us to switch between manual and autonomous modes. Using an ultrasonic sensor, NavX could detect nearby objects, stop, and decide whether to turn or reverse based on the surroundings.

For object detection, we used Python and the YOLOv8 model to process live images from the ESP32 camera. While we initially planned to train our own model, hardware limitations led us to use pre-trained YOLOv8 for real-time object identification. Images were captured at intervals and fed into the model, displaying bounding boxes and labels via a live feed.

This project gave us hands-on experience in robotics, sensor integration, real-time processing, and machine learning—building both our technical confidence and a working autonomous robot.

Results
======
NavX performed well during our tests, with success in stopping and rerouting when encountering obstacles. While some objects—were occasionally detected a bit late, the robot still responded correctly within our set threshold of 35 cm. The object identification system using YOLOv8 worked fairly well overall, though the model occasionally misclassified items due to class confidence limitations and dataset bias.

![NavX Detection](/images/detection.png)


We faced challenges setting up Bluetooth communication and initially considered using RoboFlow for detection via API, but settled on capturing images directly from the ESP32 camera feed. This simplified integration but meant detection ran at fixed intervals rather than being triggered contextually. With more time, we would improve this by enabling object-triggered image processing and refining sensor accuracy.

In future iterations, upgrading components like the microcontroller and sensors (e.g., LIDAR) would enable us to train a custom model and improve detection accuracy. Overall, NavX was a rewarding project that helped us gain and intro with hands-on experience in robotics, real-time object detection, and system integration.

Demo 
======

Check out our NavX demo videos on Google Drive:  

We also uploaded all our code to GitHub:  

