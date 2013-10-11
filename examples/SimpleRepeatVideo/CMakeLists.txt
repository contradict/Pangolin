# Find Pangolin (https://github.com/stevenlovegrove/Pangolin)
find_package(Pangolin 0.1 REQUIRED)
find_package(Eigen REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR} ${Pangolin_INCLUDE_DIRS})
link_directories(${Pangolin_LIBRARY_DIRS})
link_libraries(${Pangolin_LIBRARIES})

add_executable(SimpleRepeatVideo main.cpp)
target_link_libraries(SimpleRepeatVideo ${Pangolin_LIBRARIES})
