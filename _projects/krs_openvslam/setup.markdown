---
title: Setup
layout: default
parent: OpenVSLAM with KRS
has_toc: false
has_children: true
nav_order: 2
---

# Setup
{: .no_toc}
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Xilinx Tools

Xilinx Vitis is needed to build acceleration kernels.
The release version `2021.2` has been used. After installing Vitis 2021.2, there is a need to apply Y2K22 patch to solve an overflow issue. Refer to Xilinx [documentation](https://support.xilinx.com/s/article/76960?language=en_US) for that. 



## ROS2

[ROS2 Rolling (Ridley)](https://docs.ros.org/en/rolling/index.html) distribution needs to be installed. Refer to the installation instructions which depends on the host OS.

**Note:** Another distribution could be used

[ros-perception/vision_opencv](https://github.com/ros-perception/vision_opencv/tree/ros2) is a needed dependency. If ROS2 is to be installed using binary packages, through the OS package manager, for example on Ubuntu, *ros-rolling-desktop* includes the components of *vision_opencv*. If ROS2 is built from sources, then one has to add *vision_opencv* to the repositories list before importing them using *vcs*

## OpenVSLAM dependencies

In addition, OpenVSLAM dependencies needs to be installed. These dependencies are:
- [Eigen](http://eigen.tuxfamily.org/): version 3.3.0 or later.
- [g2o](https://github.com/RainerKuemmerle/g2o)
- [SuiteSparse](http://faculty.cse.tamu.edu/davis/suitesparse.html)
- [FBoW](https://github.com/stella-cv/FBoW)
- [yaml-cpp](https://github.com/jbeder/yaml-cpp) : version 0.6.0 or later.
- [OpenCV](https://opencv.org/) : version 3.3.1 or later.
- [Pangolin](https://github.com/stevenlovegrove/Pangolin)

FBoW needs to be installed from source. As for the other dependencies, using the OS' package manager is preferred to handle indirect dependencies.  

More details are available [here](https://stella-cv.readthedocs.io/en/latest/installation.html)

## Additional Dependencies

- [lttng](https://lttng.org/download/) UST and Tools : version 12.3 or later

## Workspace

We provide a [ROS workspace meta repository](https://github.com/soallak/ros2_openvslam_acceleration). 

In the following commands, `cd` is relative to workspace path

- Clone
```bash
git clone https://github.com/soallak/ros2_openvslam_acceleration.git
```
- Import workspace:

  ```bash 
  cd src
  vcs import < .repos
  ```

- Install colcon KRS extensions:

  ```bash
  cd src/kria/colcon-acceleration
  python setup.py build
  python setup.py install --user
  ```
  ```bash
  cd src/kria/colcon-mixin
  python setup.py build
  python setup.py install --user
  ```

### Configure environment variables

The preferred way is to use [direnv](https://direnv.net/). Therefore, it needs to be installed and hooked to the used shell.

The workspace repository's file `.envrc` need to be adjusted depending on the system configuration. This mainly consists  of changing `ROS2_HOME` and `XILINX_ROOT` variables to point to the installation paths of ROS2 distribution and Xilinx tools. 

After adjusting `.envrc`, run 
```
direnv allow
```

## Boards
The following boards are supported:
- [KV260](/projects/krs_openvslam/kv260)

Some additional steps specific to each board can be required. Refer to their respective pages for further information.
