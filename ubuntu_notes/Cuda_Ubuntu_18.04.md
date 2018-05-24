
# Installation

1. In software & updates, select the restricted and multiverse repositories
2. In the Additional Drivers tab in software & updates select the NVIDIA proprietary driver (390 for CUDA 9)
3. sudo apt update && sudo apt install nvidia-cuda-toolkit, or install it from the ubuntu software center.
4. CUDA requires gcc6, use update-alternatives to maintain both gcc7 and gcc6 as explained [here](Switch_Version_of_GCC_G++.md) 
