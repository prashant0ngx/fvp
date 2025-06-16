cmake_minimum_required(VERSION 3.10)

project(fvp_plugin)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Include plugin source files
add_library(fvp_plugin SHARED
  fvp_plugin.cpp
  ../lib/src/callbacks.cpp
)

# Find Android log and android native libraries
find_library(log-lib log)
find_library(android-lib android)
find_library(cpp_shared-lib c++_shared REQUIRED)

# Include mdk-sdk headers (update this path if needed)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/mdk-sdk/include)

# Import prebuilt native libraries from mdk-sdk
add_library(mdk SHARED IMPORTED)
set_target_properties(mdk PROPERTIES IMPORTED_LOCATION
  ${CMAKE_CURRENT_SOURCE_DIR}/mdk-sdk/lib/${ANDROID_ABI}/libmdk.so)

add_library(ffmpeg SHARED IMPORTED)
set_target_properties(ffmpeg PROPERTIES IMPORTED_LOCATION
  ${CMAKE_CURRENT_SOURCE_DIR}/mdk-sdk/lib/${ANDROID_ABI}/libffmpeg.so)

add_library(ass SHARED IMPORTED)
set_target_properties(ass PROPERTIES IMPORTED_LOCATION
  ${CMAKE_CURRENT_SOURCE_DIR}/mdk-sdk/lib/${ANDROID_ABI}/libass.so)

# Link all libraries to your plugin
target_link_libraries(fvp_plugin
  ${log-lib}
  ${android-lib}
  ${cpp_shared-lib}
  mdk
  ffmpeg
  ass
)
