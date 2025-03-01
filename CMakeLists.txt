cmake_minimum_required(VERSION 3.10)
cmake_policy(SET CMP0079 NEW)
project(HDF5_Tracker)

#-----------------------------------------------------------------------------
# Define Options
#-----------------------------------------------------------------------------
option(VOL_TRACKER "Build VOL Tracker" ON)
option(VFD_TRACKER "Build VFD Tracker" ON)
option(BUILD_C_TESTS "Build tests with C code" OFF)
option(BUILD_PY_TESTS "Build tests with python code" OFF)
option(HERMES "Running without Hermes hrun daemon" OFF)
# option(MIO "Running with mio library" OFF)
option(MMAP_IO "Running with memory mapping IO" OFF)

# Set a default build type if none was specified
set(default_build_type "Release")

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
  set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE
      STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
    "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

#-----------------------------------------------------------------------------
# Compiler Optimization
#-----------------------------------------------------------------------------
set(CMAKE_CXX_STANDARD 17)


if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    message("IN DEBUG MODE")
else()
    message("IN RELEASE MODE")
endif()

#-----------------------------------------------------------------------------
# Find Packages
#-----------------------------------------------------------------------------
# Find HDF5 with a required version of 1.14
find_package(HDF5 1.14 REQUIRED COMPONENTS C CXX HL)

if (HDF5_FOUND)
    include_directories(${HDF5_INCLUDE_DIRS})
    link_directories(${HDF5_LIBRARY_DIRS})
endif()

#-----------------------------------------------------------------------------
# Build HDF5_Tracker Packages
#-----------------------------------------------------------------------------
# Each package is built in its own directory due to using C & C++
# Option for VOL_TRACKER
if(VOL_TRACKER)
    add_subdirectory(src/vol)
endif()

# Option for VFD_TRACKER
if(VFD_TRACKER)
    add_subdirectory(src/vfd)
endif()

# if(MIO)
#     add_subdirectory( src/vfd/mio)
#     target_link_libraries(h5vfd_tracker PUBLIC mio::mio)
#     # target_link_libraries(h5vfd_tracker PUBLIC <mio::mio | mio>)
# endif()
