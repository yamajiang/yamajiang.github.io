---
title: "MATE ROV"
excerpt: "A leak sensor for MATE ROV’s enclosure to detect water leakage, improving underwater safety with real-time alerts. Created a terminal user interface to display sensor data on the flight control system for clear, immediate feedback to operators. <br/><img src='/images/leaksensor.png'  width='300'>"
collection: portfolio
---

The MATE ROV Leak Sensor & Terminal UI project is a real-time monitoring system designed for the MATE ROV, combining an Arduino-based water leak detector with a Python terminal interface. The goal of this project was to increase the safety and durability of the ROV during underwater missions by immediately alerting operators to any signs of leakage inside the enclosure.

This project enhanced my understanding of sensor integration, serial communication, and building responsive user interfaces in constrained environments like terminal-based systems.

<img src="/images/leaksensor.png" alt="ROV Leak Sensor and TUI" width="500">

Tools:
===
Arduino (C++): Used to interface with the digital water leak sensor. The code continuously reads sensor input, activates an LED, and sends real-time status messages over serial communication.

Python: For developing the terminal-based user interface using the curses and pyserial libraries.

PySerial: Allowed seamless communication between the Arduino and the Python script, enabling real-time data transfer.

Curses: Used to build an interactive terminal UI for the flight control system, displaying leak and temperature data with color-coded status updates.

Implementation:
===
Leak Sensor Firmware:
I programmed the Arduino to read input from a digital leak sensor. When water is detected, it triggers an LED and sends a serial message ("Leak detected!" or "No leak detected"). This makes it simple to interpret from the receiving system.

Terminal User Interface:
The Python TUI reads serial messages and displays the status of up to four sensors. Operators can toggle sensor monitoring in real-time and receive instant visual alerts when leaks are detected.

Summary:
===
Through this project, I gained valuable experience in low-level hardware communication and real-time interface design. I learned how to bridge sensor data from embedded systems into meaningful operator feedback, an essential component in robotics and underwater exploration. This work is a foundational step in designing resilient and responsive control systems for mission-critical robotics platforms.