# Object tracking with Lucas–Kanade, Farnebäck, Mean/CAMShift, and OpenCV tracker APIs (KCF/MIL/TLD)

### In this repository
- Lucas-Kanade Optical Flow
- Gunnar-Farneback Optical Flow
- Mean-shift Tracking
- CAMShift Tracking
- Testing some tracking APIs, such as MIL, KCF, and TLD algorithm

<br>

## Overview
This project aims to use various tracking algorithms to track objects within a video. The project will explore different optical flow algorithms such as Lucas-Kanade and Gunnar-Farneback, as well as Mean-shift and CAMShift tracking algorithms. These algorithms are used to estimate the motion of objects in a video by analyzing consecutive frames. The Lucas-Kanade method tracks the movement of feature points, while Gunnar-Farneback estimates the optical flow for a dense grid of points. Mean-shift and CAMShift are both used for object tracking by finding the object's color histogram in each frame. The project will also test some tracking APIs, such as MIL, KCF, and TLD algorithm, which are pre-trained models that can be used for object tracking. The goal of the project is to compare the performance of these various tracking algorithms and APIs and to choose the best one for a specific use case.

<br>

## Getting Started
#### 1. Fork and clone the repository:
  ```
  * git clone git://github.com/ak811/object-tracking.git
  ```
#### 2. Import the project via any Python IDEs:
  * Install [OpenCV's extra modules](https://github.com/opencv/opencv_contrib):
  ``` 
  pip install opencv-contrib-python
  ```
  * Install [NumPy](https://github.com/numpy/numpy):
  ```
  pip install numpy
  ```  
  
<!-- View Documentation -->

