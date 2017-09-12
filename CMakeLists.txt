# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.
#下载的opencv开发包 再深一层有apk,samples,sdk,选最外层就行
set(pathToOpenCv F:/winsoft/OpenCV-android-sdk/)

cmake_minimum_required(VERSION 3.4.1)

set(CMAKE_VERBOSE_MAKEFILE on)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")
# 新建jniLibs文件夹到 src/main/下，把F:\winsoft\OpenCV-android-sdk\sdk\native\libs的东西粘贴到jniLibs文件夹
set(libs "${CMAKE_SOURCE_DIR}/src/main/jniLibs")
#这是opencv开发包的头文件
#也可以这么写：include_directories(${CMAKE_SOURCE_DIR}/src/main/cpp/include)，但是要把include文件夹粘贴到项目cpp文件夹中
include_directories(${pathToOpenCv}/sdk/native/jni/include)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.
#加opencv库
add_library(libopencv_java3 SHARED IMPORTED )
set_target_properties(libopencv_java3 PROPERTIES
                 IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_java3.so")

add_library( # Sets the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             # Associated headers in the same location as their source
             # file are automatically included.
             src/main/cpp/native-lib.cpp )


# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.
# 设置链接库
target_link_libraries( # Specifies the target library.
                       native-lib libopencv_java3

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )