---
layout: page
title: Mobile Robotics in 2D (2023)
description: Perception, planning and controls for mobile robotics
img: assets/img/robotics/robo_front.png
importance: 1
---

This project is part of the course "Introduction to Robotics" taught by professor Henrik Christensen in the Computer Science Department at UC San Diego. Link of the course is here: <http://www.hichristensen.net/CSE276A-23/index.html>

Project repository: <https://github.com/fjzs/mobile-robotics-2d>

## **1 Problem Definition**
Given a Four Mecanum Wheeled Mobile Robot, design a "Roomba"-like system for a 2 x 2 square meters zone (free of obstacles). Use 12 April Tags to localize the robot as shown in Figure 1. Provide an algorithm to clean the zone, a video of it and a performance guarantee for this coverage.

<div class="row"><div class="col-sm mt-3 mt-md-0 text-center">
    {%
        include figure.html path="assets/img/robotics/area_description.png" 
        class = "img-fluid rounded z-depth-1"
        width = 500
        caption = "Figure 1: the area to cover is a square 2x2 square meters surrounded by april tags to help localize the robot."
    %}
</div></div>

#### **1.1 What is a Four Mecanum Wheeled Mobile Robot?**
A mecanum wheel is an omnidirectional wheel design, providing the capability to move in 2D and rotate along the z axis, as opposed to traditional wheels that can only move in 1D. In this project we followed and implemented the kinematic model provided by Taheri et al (2015) (<https://research.ijcaonline.org/volume113/number3/pxc3901586.pdf>). In Figure 2 you can see the kinematic configuration of this robot. Each wheel $$i$$ has an angular velocity of $$\omega_i$$, providing the robot with a 2D velocity $$(v_x, v_y)$$ and a rotation around the z axis (yaw) given by $$\omega_z$$. 

<div class="row"><div class="col-sm mt-3 mt-md-0 text-center">
    {%
        include figure.html path="assets/img/robotics/mecanum_kinematic.png" 
        class = "img-fluid rounded z-depth-1"
        width = 800
        caption = "Figure 2: On the left you can see the kinematic configuration of this robot. Notice that the robot coordinate frame is located at the center of the robot (O), and Xr axis is towards the front of the robot, and Yr is towards the left of the robot. Source: Taheri et al (2015)"
    %}
</div></div>

#### **1.2 The particular robot used**
In this project the robot consisted in a Mbot Mega (4 mecanum wheel robot) + Qualcomm RB5 board. This robot is equipped with a monocular camera that we will use for perceptions tasks. You can see it in Figure 3.

<div class="row"><div class="col-sm mt-3 mt-md-0 text-center">
    {%
        include figure.html path="assets/img/robotics/robot_rb5.png" 
        class = "img-fluid rounded z-depth-1"
        width = 500
        caption = "Figure 3: this is the robot we used, which provides enough computing resources and a camera to perform multiple robotic tasks."
    %}
</div></div>

#### **1.3 What is an April Tag and how to use it for localization**
April Tag is a visual system developed at University of Michigan (<https://april.eecs.umich.edu/software/apriltag>). It's like a QR code that can be read by a library developed by the authors. In Figure 4 there is a description of how this works. Given the coordinate frames of the world (w), robot (r), robot's camera (c) and april tag (a), when the robot's camera reads an april tag, the library retrieves both the id of the tag and the 4x4 transformation $$^{c}T_{a}$$, which is the relative transformation that takes a point in the april tag coordinate frame to the robot's camera coordinate frame. Then, using that $$^{c}T_{a} = (^{a}T_{c})^{-1}$$, we can get the robot position (x,y,z) and orientation (roll, pitch, yaw) in the world frame using the following equation:

$$^{w}T_{r} = ^{w}T_{a} \cdot ^{a}T_{c} \cdot ^{c}T_{r}$$

<div class="row"><div class="col-sm mt-3 mt-md-0 text-center">
    {%
        include figure.html path="assets/img/robotics/april_tag.png" 
        class = "img-fluid rounded z-depth-1"
        width = 500
        caption = "Figure 4: An april tag is like a QR code that when read by the April Tag library, returns the id of the tag and the relative position and orientation of the camera with respect to the april tag. In this diagram we have 4 coordinate frames: the world frame (w), the robot frame (r), the robot's camera frame (c) and the april tag frame (a). Here we show the april tag id=0 for family 36h11 specifically, but there are multiple families and ids of april tags."
    %}
</div></div>


## **2 Solution**

#### **2.1 Architecture**