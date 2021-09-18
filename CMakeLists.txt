cmake_minimum_required(VERSION 2.8.3)
project(pangolin)

find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/pangolin)

ExternalProject_Add(pangolin_src
  GIT_REPOSITORY https://github.com/stevenlovegrove/Pangolin
  GIT_TAG v0.6	# v0.6
  PREFIX ${CMAKE_CURRENT_BINARY_DIR}/pangolin
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX} -DCMAKE_BUILD_TYPE=Release
  -DBUILD_SHARED_LIBS=ON -DBUILD_TESTS=OFF -DBUILD_TOOLS=OFF -DBUILD_EXAMPLES=OFF
)

ExternalProject_Get_Property(pangolin_src install_dir)
include_directories(${install_dir}/include)

cs_install()

install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/pangolin
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.hpp"
  PATTERN "*.h"
  PATTERN ".git" EXCLUDE
)

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include/
    LIBRARIES pangolin
    DEPENDS GLEW
)
