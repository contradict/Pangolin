cmake_minimum_required(VERSION 2.8.3)
project("pangolin")
find_package(catkin REQUIRED)

set(PANGOLIN_VERSION_MAJOR 0)
set(PANGOLIN_VERSION_MINOR 1)
set(PANGOLIN_VERSION ${PANGOLIN_VERSION_MAJOR}.${PANGOLIN_VERSION_MINOR})

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/CMakeModules/")

option(BUILD_EXAMPLES "Build Examples" OFF)

if(NOT ANDROID)
   option(BUILD_SHARED_LIBS "Build Shared Library" ON)
else()
   set(BUILD_SHARED_LIBS OFF)
   set(ANDROID_PACKAGE_NAME "com.github.stevenlovegrove.pangolin")
   include(AndroidUtils)
endif()

# Overide with cmake -DCMAKE_BUILD_TYPE=Debug {dir}
if( NOT CMAKE_BUILD_TYPE )
   message("Build type not set (defaults to release)")
   message("-DCMAKE_BUILD_TYPE=Debug for debug")
   set( CMAKE_BUILD_TYPE Release )
endif()

# Platform configuration vars
include(SetPlatformVars)

string(TOLOWER ${PROJECT_NAME} LIBRARY_NAME)

# make an uninstall target 
# configure_file(
#  	"${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
#   	"${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
#   	IMMEDIATE @ONLY
# )

catkin_destinations()
file( MAKE_DIRECTORY
    "${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_INCLUDE_DESTINATION}")

catkin_package(
    INCLUDE_DIRS
        include
        "${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_INCLUDE_DESTINATION}"
    LIBRARIES ${LIBRARY_NAME}
    DEPENDS Boost OpenGL GLEW
    )

#add_custom_target(uninstall
#  "${CMAKE_COMMAND}" -P "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake")
add_subdirectory("src")

if(BUILD_EXAMPLES)
  add_subdirectory(examples)
endif()


