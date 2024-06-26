cmake_minimum_required(VERSION 3.0.0)
project(vio VERSION 0.1.0 LANGUAGES C CXX)

# set(CMAKE_DEFAULT_BUILD_TYPE, "Release")
set(CMAKE_DEFAULT_BUILD_TYPE, "Debug")


if (NOT CMAKE_BUILD_TYPE)
    message(STATUS "Setting build type to '${DEFAULT_BUILD_TYPE}' as none was specified.")
    set(CMAKE_BUILD_TYPE "${DEFAULT_BUILD_TYPE}" CACHE
            STRING "Choose the type of build." FORCE)
    # Set the possible values of build type for cmake-gui
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
            "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif ()

set(CMAKE_CXX_FLAGS "-std=c++11 -g -Wall")

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED True)


FIND_PACKAGE( OpenMP REQUIRED)
if(OPENMP_FOUND)
    message("OPENMP FOUND")
    ADD_DEFINITIONS(-DUSE_OPENMP)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} -Wno-reorder" CACHE STRING "" FORCE)
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS} -DNDEBUG -Wno-reorder -O2" CACHE STRING "" FORCE)



list(APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")

option(BUILD_APPS "Build APPs for slam course" YES)
option(BUILD_TESTS "Build test for slam course" No)

# third party libs
# eigen
find_package(Eigen REQUIRED)
include_directories(${EIGEN_INCLUDE_DIR})
message("EIGEN_INCLUDE_DIR: ${EIGEN_INCLUDE_DIR}")

# ceres
find_package(Ceres REQUIRED)
include_directories(${CERES_INCLUDE_DIRS})
message("CERES_INCLUDE_DIRS: ${CERES_INCLUDE_DIRS}")

# opencv
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIR})
message("OpenCV_INCLUDE_DIRS: ${OpenCV_INCLUDE_DIRS}")

# glog
find_package(Glog REQUIRED)
include_directories(${PACKAGE_PREFIX_DIR}/include/glog)
message("GLOG_INCLUDE_DIRS: ${PACKAGE_PREFIX_DIR}/include/glog")

# vio backend
include_directories(${PROJECT_SOURCE_DIR})

add_subdirectory(backend)

if (BUILD_APPS)
    add_subdirectory(app)
endif ()
