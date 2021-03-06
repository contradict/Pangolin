# Find Pangolin (https://github.com/stevenlovegrove/Pangolin)
find_package(Pangolin 0.1 REQUIRED)
find_package(Eigen REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR} ${Pangolin_INCLUDE_DIRS})
link_directories(${Pangolin_LIBRARY_DIRS})
link_libraries(${Pangolin_LIBRARIES})

find_package(CUDA QUIET)

if(CUDA_FOUND)
  cuda_include_directories(${CMAKE_CURRENT_SOURCE_DIR})

  set(CUDA_NVCC_FLAGS "-O3")
  add_definitions(-DCUDA_BUILD)

  if(CUDA_SDK_ROOT_DIR AND EXISTS("${CUDA_SDK_ROOT_DIR}/lib/libcutil_x86_64.a") )
    add_definitions(-DUSE_CUTIL)
    include_directories( ${CUDA_SDK_ROOT_DIR}/common/inc )
    link_directories( ${CUDA_SDK_ROOT_DIR}/lib )
  endif()

  cuda_add_executable(
    VBODisplay
    main.cpp kernal.cu
  )

  if(CUDA_SDK_ROOT_DIR AND EXISTS("${CUDA_SDK_ROOT_DIR}/lib/libcutil_x86_64.a") )
    target_link_libraries(VBODisplay libcutil_x86_64.a)
  endif()
endif()
