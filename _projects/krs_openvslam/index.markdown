---
title: OpenVSLAM with KRS
layout: default
has_children: true
has_toc: true
---

The [Kria Robotics Stack](https://xilinx.github.io/KRS/sphinx/build/html/index.html) (KRS) developed by Xilinx provides integration of Vitis acceleration kernels within ROS2 framework. This is achieved by extending ROS2 tooling, namely colcon and ament. 

In this work, the goal of this project is to showcase the usage of KRS in accelerating and improving the performance of applications. To demonstrate this, a SLAM pipeline based on [OpenVSLAM](https://github.com/soallak/openvslam) is used. We want to show a whole visual odometry pipeline which consists of image acquisition, image preprocessing and finally pose estimation. 
