---
title: Build
layout: default
parent: OpenVSLAM with KRS
has_toc: true
nav_order: 4
---

# Build
{: .no_toc}
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

**Note:** For all build commands, the workspace overlay should not be sourced

**Note:** zsh has been used. Change any `setup.zsh` occurence with `setup.$SHELL` wherever it is needed.

## Host Build

This builds the ROS2 workspace. It always necessary as it also creates the `acceleration` directory that is needed for *Target Builds*.


```bash
source $ROS2_HONE/setup.zsh 
colcon build --merge-install --cmake-args -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DCMAKE_BUILD_TYPE=Release --parallel-workers 32
```

## Target Build

This builds the workspace for the target board. The target board selection is done through `colcon acceleration select` command.

### KV260

```bash
source $ROS2_HONE/setup.zsh 
colcon acceleration select kv260-soallak
colcon build --build-base=build-kv260-soallak --install-base=install-kv260-soallak --merge-install --mixin kv260-soallak --cmake-args -DROS_VITIS=ON -DCMAKE_BUILD_TYPE=Release -DNO_IMAGE_PROC_KERNEL=ON
```

The `-DNO_IMAGE_PROC_KERNEL=ON` cmake argument is used to disable HW Compilation of `image_proc` kernels. These include rectification and resize kernels which are not needed for the OpenVSLAM demonstration currently.


### Disable All Kernels

This done by passing `-DNOKERNELS=ON` to the build command, for example, for KV260:

```bash
colcon build --build-base=build-kv260-soallak --install-base=install-kv260-soallak --merge-install --mixin kv260-soallak --cmake-args -DROS_VITIS=ON -DCMAKE_BUILD_TYPE=Release -DNOKERNELS=ON
```

Sometimes, there is no need to re-trigger Vitis. This is not detected automatically, that is why `-DNOKERNELS=ON` was introduced to reduce drastically the compilation time when *FPGA* related code has not been changed. 
