---
title: "Face Guard"
excerpt: "A privacy-focused tool that uses OpenCV and MediaPipe to blur faces in photos, videos, and live webcam feeds. It leverages real-time facial landmark detection to ensure consistent and accurate anonymization."
collection: portfolio
---

Face Guard is a privacy-focused project that utilizes OpenCV and MediaPipe to blur faces in images, videos, and live webcam streams. This project marked my first experience working with face detection and anonymization, where I implemented real-time image processing techniques to protect user privacy. By applying transformations to images and video frames, I ensured that sensitive face data was properly anonymized across various media formats.


<img src="/images/faceguard.png" alt="Face Guard" width="500">


Tools & Implementation:
====
OpenCV: A powerful computer vision library that I used for image and video handling, including reading input files, processing frames, and saving the output.

MediaPipe: A framework that I used for face detection. The Face Detection module is used to identify faces in an image or video frame, where I applied a blur effect on detected faces to anonymize them.

Python: I wrote the implementation in Python, utilizing argument parsing to easily switch between different modes of operation (image, video, or webcam feed).

Implementation:
====
I created a function (process_img) that accepts an image and the face detection model. It detects faces in the image and applies a blur to the regions where faces are detected. The blur intensity can be adjusted to suit the privacy requirements.

Image Mode: For processing a single image, I read the image from the specified path, processed it for face anonymization, and saved the result.

Video Mode: I used OpenCV's VideoCapture to read frames from a video file, processed each frame for face detection, and saved the output to a new video file.

Webcam Mode: The program can also process live webcam feeds. It continuously captures frames from the webcam, processes them for face anonymization, and displays the output in real-time. The feed stops when the user presses the 'q' key.

Through this project, I gained hands-on experience with OpenCV's powerful image processing capabilities and MediaPipe's ease of use for real-time applications. I also developed a deeper understanding of the challenges in ensuring privacy in digital media. Face Guard sparked my interest in privacy-focused technology and provided a strong foundation for future work in computer vision, interactive applications, and real-time data processing."