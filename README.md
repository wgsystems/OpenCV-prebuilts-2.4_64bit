# OpenCV-prebuilts-2.4_64bit
jniLibs prebuilts for OpenCV 2.4 compiled for arm64 and x86_64. 
opencv/opencv branch 2.4 as of 2019-12-13.
Can be easily dropped into existing prebuilt sdk with only x86 platform. 

# Introduction
OpenCV is great when it comes to computer vision, even in mobile apps.
However since the August 1, 2019 the Google Play Store only accepts apk's with native code if the 64 bit architecures are included.
Since there were are no official prebuilts for OpenCV 2.4 in 64 bits, I needed to build them myself. Because this was kinda tricky, here's a little Nice-To-Know gist.

# Requirements
Get the latest Android NDK (for me that was version 20)
Android SDK is *not* required if you don't need to have the java examples.
Linux (I used Ubuntu 18.04, you can use every other distro but 18.04 has like the "repo-version-sweet-spot")
OpenCV in the 2.4 branch (git clone https://github.com/opencv/opencv -b 2.4)

# Let's build
-> Forget that nonsense-stuff out of the opencv wiki, it's outdated. We won't even use their toolchain but the NDK's one as the OpenCV one does not support multiarch-64 in any way.

I saved my opencv repo on my desktop and created two more build folders on the desktop (build-arm64, build-x86_64).
My example here is for arm64 (arm64-v8a) but if you change the -DANDROID_ABI flag in the following cmake command, you can easily build for x86 aswell.

*Inside build-arm64*

```
cmake -DCMAKE_BUILD_WITH_INSTALL_RPATH=ON \
        -DCMAKE_TOOLCHAIN_FILE=/home/parallels/Android/Ndk/build/cmake/android.toolchain.cmake \
        -DANDROID_SDK=/home/parallels/Android/Sdk -DANDROID_NDK=/home/parallels/Android/Ndk/ \
        -DANDROID_ABI="arm64-v8a" \
        -DBUILD_FAT_JAVA_LIB=ON \
        -DBUILD_opencv_androidcamera=ON \
        -DBUILD_OPENCV_NATIVECAMERA=ON \
        -DANDROID_NATIVE_API_LEVEL=22 \
        -DBUILD_DOCS=OFF \
        -DBUILD_PERF_TESTS=OFF \
        -DBUILD_TESTS=OFF \
../opencv/

make install/strip
```

This will give you everything you need to develop Android apps inside the build-arm64/install folder. You can omitt the 
.a files in the lib folder.


