# CL_Stub

| Android | Linux | macOS |
| --- | --- | --- |
| [![CircleCI](https://circleci.com/gh/csarron/cl_stub/tree/master.svg?style=shield)](https://circleci.com/gh/csarron/cl_stub/tree/master) | [![Run Status](https://api.shippable.com/projects/5ac96f0b03be900700447aa0/badge?branch=master)](https://app.shippable.com/github/csarron/cl_stub) | [![Build Status](https://travis-ci.org/csarron/cl_stub.svg?branch=master)](https://travis-ci.org/csarron/cl_stub) |

For easy use of OpenCL drivers by building a shared stub library, which is especially useful for Android due to lack of Google's official support. For phones not having OpenCL drivers, you may find [the other project](https://github.com/csarron/PhoneVendorBlobs) useful.

## Usage
See [Project Integration](#project-integration)
### Android
1. Build
- configure NDK environment:
`export ANDROID_NDK=/path/to/your/ndk/`, e.g. `export ANDROID_NDK=/opt/android-ndk-r16b`
- arm:
`mkdir build_arm; cd build_arm`;
`cmake -DCMAKE_TOOLCHAIN_FILE=$ANDROID_NDK/build/cmake/android.toolchain.cmake -DANDROID_ABI="armeabi-v7a" -DANDROID_PLATFORM=android-17 ..`;
`make`
- arm64:
`mkdir build_arm64; cd build_arm64`;
`cmake -DCMAKE_TOOLCHAIN_FILE=$ANDROID_NDK/build/cmake/android.toolchain.cmake -DANDROID_ABI="arm64-v8a" -DANDROID_PLATFORM=android-17 ..`;
`make`
2. Run
push the library and executable to the phone by adb: `adb push test_stub /data/local/tmp` and then run `adb shell /data/local/tmp/test_stub`, below are two sample outputs:

```
open OpenCL library at /system/lib/egl/libGLES_mali.so
Using platform: ARM Platform
Found device: Mali-T880 5MHz
Using device: Mali-T880
result: {19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 }

open OpenCL library at /system/lib64/libOpenCL.so
Using platform: QUALCOMM Snapdragon(TM)
Found device: QUALCOMM Adreno(TM) OpenCL 2.0 Adreno(TM) 540 1MHz
Using device: QUALCOMM Adreno(TM)
result: {19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 }
```

### macOS

Just build `mkdir build && cd build && cmake .. && make` and run `./test_stub`
sample output:

```
open OpenCL library at /System/Library/Frameworks/OpenCL.framework/OpenCL
Using platform: Apple
Found device: Intel(R) Core(TM) i7-3720QM CPU @ 2.60GHz OpenCL 1.2  2600MHz
Found device: HD Graphics 4000 OpenCL 1.2  1250MHz
Using device: HD Graphics 4000
result: {19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 }
```

### Linux

**Note**: you may need to install OpenCL driver on Linux, try: `sudo apt update &&
sudo apt install ocl-icd-opencl-dev`

Just build `mkdir build && cd build && cmake .. && make` and run `./test_stub`
sample output:

```
open OpenCL library at /usr/lib/x86_64-linux-gnu/libOpenCL.so
Using platform: NVIDIA CUDA
Found device: GeForce GTX 1070 OpenCL 1.2 CUDA 1771MHz
Found device: GeForce GTX 1070 OpenCL 1.2 CUDA 1683MHz
Using device: GeForce GTX 1070
result: {19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 19 }
```

## Project Integration
- [x] library use, just copy libcl_stub.so and correspoding header files (`include/`)
- [ ] cmake integration: todo testing.


## Credit
Examples in test_stub.cc are borrowed from [OpenCL-examples](https://github.com/Dakkers/OpenCL-examples/blob/master/example00/main.cpp)


