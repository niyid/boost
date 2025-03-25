cmake_minimum_required(VERSION 3.5...3.16)

# Set build type before project()
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR AND NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

# Android-specific Boost configuration - using correct BOOST_ prefix
set(BOOST_ROOT "/home/user/android_boost/armeabi-v7a" CACHE PATH "Boost installation root")
set(BOOST_INCLUDEDIR "${BOOST_ROOT}/include" CACHE PATH "Boost includes")
set(BOOST_LIBRARYDIR "${BOOST_ROOT}/lib" CACHE PATH "Boost libraries")

# Boost find_package variables - these use mixed case Boost_ prefix
set(Boost_NO_SYSTEM_PATHS TRUE)
set(Boost_USE_STATIC_LIBS TRUE)
set(Boost_USE_STATIC_RUNTIME TRUE)

project(Boost VERSION 1.88.0 LANGUAGES CXX)

# Debug output for verification
message(STATUS "Boost configuration:")
message(STATUS "  BOOST_ROOT = ${BOOST_ROOT}")
message(STATUS "  BOOST_INCLUDEDIR = ${BOOST_INCLUDEDIR}")
message(STATUS "  BOOST_LIBRARYDIR = ${BOOST_LIBRARYDIR}")

# Find Boost components
set(Boost_DEBUG TRUE)  # Enable detailed search logging
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
    message(STATUS "Found Boost ${Boost_VERSION} at ${Boost_INCLUDE_DIRS}")
    message(STATUS "Boost libraries: ${Boost_LIBRARIES}")
else()
    message(FATAL_ERROR "Boost NOT found. Please verify:"
        "\n1. The libraries exist in ${BOOST_LIBRARYDIR}"
        "\n2. The files are named correctly (libboost_*.a)"
        "\n3. The architecture (armeabi-v7a) matches your toolchain"
    )
endif()

# Additional Android-specific settings
if(ANDROID)
    add_definitions(-DBOOST_ASIO_DISABLE_STD_ALIGNED_ALLOC)
    add_definitions(-DBOOST_UUID_DISABLE_ALIGNMENT)
endif()
