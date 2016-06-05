# librealsense with ARM support

This repository is a fork of the original [librealsense](https://github.com/IntelRealSense/librealsense) library with the addition of ARM support. Librealsense uses SSSE3 SIMD instructions for unpacking frames to the desired color format. This was necessary probably due to the nature of the pixel unpacking operations as they are inherently slow and are data parallel. Since SSSE3 is specific to x86 architecture, workarounds are needed for getting the code to compile under ARM. 

My goal was to be able to use RealSense R200 sensor with NVIDIA Jetson TX1. So I took a shortcut and replaced all SSSE3 instructions with regular `for` loops. Ideally, these instructions should be rewritten with NEON for optimal performance, but on my Jetson TX1 I can have all streams enabled and maintain 60 FPS. This fork contains the modifications necessary to get librealsense to work under ARM. I have NOT tested every feature of the library and I don't provide ANY guarantee for functionality (neither written nor implied). Users have reported this to work with Jetson TK1 too but I'm not sure if TK1 can also maintain 60 FPS with all streams enabled.

I will try to keep this fork up to date with the original repository as best as I can. The original discussion thread that lead to this fork is [located here](https://github.com/IntelRealSense/librealsense/issues/73).

## Build Instructions
Comprehensive platform-specific installation instructions are [available here](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md). This fork needs to be build with the LIBUVC backend. Therefore, build with:

    make BACKEND=LIBUVC -j4

The latest kernels on Jetson TX1 boards do not support the Video4Linux backend. I have tried the upstream linux kernel on the TX1 but without the necessary NVIDIA drivers, I do not think there is a way to get the V4L backend to work. Nevertheless, there have been [reports of people successfully using the V4L backend](https://github.com/Maghoumi/librealsense/pull/2).

## Troubleshooting
Every time the video streams are opened, the USB port needs to be reset. So far, I haven't found an elegant way of resetting the USB port via software and I had to resort to physically unplugging/replugging the USB3 port after every use. If you've been able to build this successfully under ARM but have problems getting the examples to work, try unplugging/replugging your RealSense sensor a few times. Also, running the examples with `sudo` privileges will output a more detailed error code when the examples fail to run.

## Contribution and Support
If you have fixed an issue, do not hesitate to submit pull requests! They are greatly appreciated.
If you find this repository useful, kindly give it stars! It helps others to know this code works.

## Acknowledgement
I would like to thank @jetsonhacks for fixing Makefile issues with Jetson TK1 boards.

## License
Refer to the [original repository](https://github.com/IntelRealSense/librealsense) for licensing information.
