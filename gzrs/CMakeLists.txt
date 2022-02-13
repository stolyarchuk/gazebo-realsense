cmake_minimum_required(VERSION 3.1 FATAL_ERROR)

add_library(RealSensePlugin SHARED RealSensePlugin.cc)
target_link_libraries(RealSensePlugin ${GAZEBO_LIBRARIES})

install(TARGETS RealSensePlugin DESTINATION ${GZRS_PLUGIN_PATH})

