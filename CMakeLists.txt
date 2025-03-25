# Copyright 2019, 2021 Peter Dimov
# Distributed under the Boost Software License, Version 1.0.
# See accompanying file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt

cmake_minimum_required(VERSION 3.5...3.16)

# The default build type must be set before project()
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR AND NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

# Set BoostRoot and related paths explicitly
set(BoostRoot "/home/user/android_boost/armeabi-v7a" CACHE PATH "Root directory of Boost installation")
set(BOOST_ROOT "${BoostRoot}" CACHE PATH "Alternative variable for Boost root directory")
set(BOOST_INCLUDEDIR "${BoostRoot}/include" CACHE PATH "Boost include directory")
set(BOOST_LIBRARYDIR "${BoostRoot}/lib" CACHE PATH "Boost library directory")

# Ensure these variables are available to subdirectories and find_package
set(BOOST_NO_SYSTEM_PATHS ON) # Prevent CMake from searching system paths
set(BOOST_USE_STATIC_LIBS ON) # Force static linking (common for Android)

project(Boost VERSION 1.88.0 LANGUAGES CXX)

set(BOOST_SUPERPROJECT_VERSION ${PROJECT_VERSION})
set(BOOST_SUPERPROJECT_SOURCE_DIR ${PROJECT_SOURCE_DIR})

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/tools/cmake/include)

# Verify paths before including BoostRoot
message(STATUS "BoostRoot = ${BoostRoot}")
message(STATUS "Boost_INCLUDEDIR = ${Boost_INCLUDEDIR}")
message(STATUS "Boost_LIBRARYDIR = ${Boost_LIBRARYDIR}")

# Standard Boost detection
find_package(Boost REQUIRED COMPONENTS 
    system 
    filesystem 
    thread 
    date_time 
    chrono 
    regex 
    serialization 
    program_options 
    locale
)

if(Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIRS})
    message(STATUS "Boost libraries: ${Boost_LIBRARIES}")
endif()
