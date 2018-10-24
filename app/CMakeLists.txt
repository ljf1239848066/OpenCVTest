# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

set(CMAKE_VERBOSE_MAKEFILE on)
set(libs "${CMAKE_SOURCE_DIR}/src/main/jniLibs")

include_directories(${CMAKE_SOURCE_DIR}/src/main/jni/include)
include_directories(${CMAKE_SOURCE_DIR}/src/main/jni)

#--------------------------------------------- import ---------------------------------------------#
add_library(libopencv_java3
        SHARED
        IMPORTED)
set_target_properties(libopencv_java3
        PROPERTIES IMPORTED_LOCATION
        ${libs}/${ANDROID_ABI}/libopencv_java3.so)

#add_library(opencvlib_java
#        SHARED
#        IMPORTED)
#set_target_properties(opencvlib_java
#        PROPERTIES IMPORTED_LOCATION
#        ${libs}/${ANDROID_ABI}/libopencv_java.so)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")

add_library(#
        detection_based_tracker
        SHARED
        ${CMAKE_SOURCE_DIR}/src/main/jni/DetectionBasedTracker_jni.cpp)


find_library( # Sets the name of the path variable.
        log-lib
        log)

target_link_libraries( # Specifies the target library.
        detection_based_tracker
        libopencv_java3
        -landroid #native_window
        -ljnigraphics #bitmap
        -lOpenSLES #openSLES
        # Links the target library to the log library
        # included in the NDK.
        ${log-lib})