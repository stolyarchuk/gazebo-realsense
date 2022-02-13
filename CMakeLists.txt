cmake_minimum_required(VERSION 3.1 FATAL_ERROR)

project(gazebo-realsense LANGUAGES CXX)
include(FindPkgConfig)

# try to use ccache if found
find_program(CCACHE_PROGRAM ccache)
if(CCACHE_PROGRAM)
  set(CMAKE_CXX_COMPILER_LAUNCHER ${CCACHE_PROGRAM})
endif(CCACHE_PROGRAM)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

# Custom instalation paths
if (GZRS_PLUGIN_INSTALL_PATH)
    GET_FILENAME_COMPONENT(GZRS_PLUGIN_PATH ${GZRS_PLUGIN_INSTALL_PATH} ABSOLUTE)
else()
    set(GZRS_PLUGIN_PATH ${GAZEBO_PLUGIN_PATH})
endif()

# Gazebo 7 does not provide a ${GAZEBO_MODEL_PATH} variable.
# That's why we are accessing it relatively to ${GAZEBO_MEDIA_PATH}.
# A proper ${GAZEBO_MODEL_PATH} variable might be available in Gazebo 8+.
if (GZRS_MODEL_INSTALL_PATH)
    GET_FILENAME_COMPONENT(GZRS_MODEL_PATH ${GZRS_MODEL_INSTALL_PATH} ABSOLUTE)
elseif(GAZEBO_MODEL_PATH)
    set(GZRS_MODEL_PATH ${GAZEBO_MODEL_PATH})
else()
    GET_FILENAME_COMPONENT(GZRS_MODEL_PATH ${GAZEBO_MEDIA_PATH}/../models ABSOLUTE)
endif()

if(NOT OGRE_PREFIX)
  set(OGRE_PREFIX /usr/local/ogre)
endif()

if(NOT IGNITION_PREFIX)
  set(IGNITION_PREFIX /usr/local/ignition)
endif()

if(NOT GAZEBO_PREFIX)
  set(GAZEBO_PREFIX /usr/local/gazebo)
endif()

if(NOT FFMPEG_PREFIX)
  set(FFMPEG_PREFIX /usr/local/ffmpeg)
endif()

list(APPEND CMAKE_PREFIX_PATH ${OGRE_PREFIX} ${IGNITION_PREFIX} ${GAZEBO_PREFIX} ${FFMPEG_PREFIX})

pkg_check_modules(FFMPEG REQUIRED libavcodec)
pkg_check_modules(OGRE REQUIRED OGRE OGRE-Terrain)
pkg_check_modules(GAZEBO REQUIRED gazebo)

# Include and Link directories
include_directories(
  ${GAZEBO_INCLUDE_DIRS}
  ${FFMPEG_INCLUDE_DIRS}
  ${OGRE_INCLUDE_DIRS})

link_directories(
  ${GAZEBO_LIBRARY_DIRS}
  ${FFMPEG_LIBRARY_DIRS}
  ${OGRE_LIBRARY_DIRS})

add_subdirectory(gzrs)
add_subdirectory(models)
