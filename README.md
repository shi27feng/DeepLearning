This repository will mostly contain documentation about my Tensorflow (CPU + GPU) setup incl. the prerequirements on (L)Ubuntu Linux.

# Hardware Setup #
- Board: Asrock Fatal1ty X370 Gaming-ITX/ac (Bios 3.40)
- CPU: AMD A12-9800E APU (4 CPU cores and 8 GPU cores [= 512 stream processors])
- CPU Cooler: Noctua NH-L9a incl. AM4 retention kit
- RAM: 2x 8 GB DDR4 2400 Kingston HyperX
- Video Card: integrated in CPU, currently only 512 MB set as shared video memory due to the Bios but this will change with new 4.xx Bios versions, then up to 2048 MB shared video memory is usable
- Storage: 256 GB Samsung 950 Pro M.2 SSD
- Power Supply: Corsair SF450 SFX
- Case: Cooltek U1

# Basic Software Setup #
- pure UEFI install
- Lubuntu 17.10
- Kernel: the current version of the 4.14.x branch (via [Ubuntu Mainline Kernels](http://kernel.ubuntu.com/~kernel-ppa/mainline/)) and the official 4.13 Ubuntu 17.10 version as fallback
- Filesystem: DM-LUKS encrypted SSD with BTRFS filesystem
- Desktop Environment: LXDE

# AMD OpenCL Setup #
- see [AMD_OpenCL_Setup](https://github.com/AlphasCodes/DeepLearning/blob/master/AMD_OpenCL_Setup.md)

# Tensorflow Setup #
- see [Tensorflow Setup](https://github.com/AlphasCodes/DeepLearning/blob/master/Tensorflow_Setup.md)
