---
title: Run
layout: default
parent: OpenVSLAM with KRS
has_toc: true
nav_order: 5
---

# Run
## Full Pipeline On Host

In this particular case, there is no need to build nor deploy the ROS2 overlay for a particular edge target. 

After building the ROS2 OpenVSLAM Acceleration overlay:

```bash
source $ROS2_HONE/setup.zsh 
source install/setup.zsh

ros2 launch slam_launch slam_full.launch.py dataset_path:=build/data_euroc/Machine_Hall_01/ dataset_period:=100 dataset_type:=<dataset-path>  system_type:=stereo start_pangolin_viewer:=true start_rviz2:=true

```

By default, the first sequence of the EuRoC MAV dataset will be downloaded. this can be found in `build/data_euroc/Machine_Hall_01/` relatively to the workspace.


## On Target

### Deployment

The deployment consist of the copying the ROS2 overlay to the board. Which for KV260 target can be done using: 
```
scp -r install-kv260-soallak kv260:
```

In addition, EuRoC MAV sequences needs to be available on the target board. One approach is to copy a dataset sequence to a USB Drive. For example to copy the sequence downloaded by default: 

```bash
cp -r build/data_euroc/Machine_Hall_01 <usb-mount-point>/
```

Finally, plug-in the USB drive and login using e.g. `ssh kv260`. The last steps, on the KV260 target board, are:
1. Mount USB drive
2. Install firmware:
    ```bash
    cp -r install-kv260-soallak/lib/hwcv_kernels/ /lib/firmware/xilinx/
    ```
3. Program FPGA:
    ```bash
    xlnx-config -x loadapp hwcv_kernels
    ```
4. Test accelerated kernels:
    ```bash
    source /opt/ros/foxy/setup.bash
    source install-kv260-soallak/local_setup.bash 
    /install-kv260-soallak/lib/hwcv/test_all
    ```

### Partial Pipeline

In this setup, only the accelerated stereo matching is computed on the target board. Everything else is runs on the Host PC.

On host:

```bash
source $ROS2_HONE/setup.zsh 
source install/setup.zsh
ros2 launch slam_launch slam_host.launch.py dataset_path:=build/data_euroc/Machine_Hall_01/ dataset_period:=500 dataset_type:=euroc start_rviz2:=true start_pangolin_viewer:=true
```

On KV260 Target:

```bash
source /opt/ros/foxy/setup.bash
source install-kv260-soallak/local_setup.bash 
ros2 launch slam_launch slam_edge.launch.py stereo_algorithm:=2
```

### Full Pipeline

In this setup, image acquisition, rectification and accelerated stereo matching is done on the target KV260 board, while the Host PC does the poses estimation and visualization.

On Host:

```bash
source $ROS2_HONE/setup.zsh 
source install/setup.zsh
ros2 launch slam_launch slam_full_host.launch.py system_type:=stereo-depth start_rviz2:=true start_pangolin_viewer:=true
```

On Target: 

```bash
source /opt/ros/foxy/setup.bash
source install-kv260-soallak/local_setup.bash 
ros2 launch slam_launch slam_full_edge.launch.py stereo_algorithm:=2 dataset_path:=<dataset-path> dataset_period:=200 dataset_type:=euroc
```