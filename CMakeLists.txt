cmake_minimum_required(VERSION 2.8.3)
project(sophus_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple()

include(ExternalProject)

set(SOPHUS_CATKIN_INCLUDE ${CATKIN_DEVEL_PREFIX}/include/sophus)
file(MAKE_DIRECTORY ${SOPHUS_CATKIN_INCLUDE})

SET(SOPHUS_CXX_FLAGS "${SOPHUS_CXX_FLAGS} -std=c++11")

ExternalProject_Add(sophus_src
  GIT_REPOSITORY  https://github.com/luhongquan66/Sophus.git
  GIT_TAG master
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND cmake -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX}
  -DCMAKE_BUILD_TYPE=Release ../sophus_src
  BUILD_COMMAND make -j8
  INSTALL_COMMAND make install
)

cs_install()

install(DIRECTORY ${SOPHUS_CATKIN_INCLUDE}
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.hpp"
)

#为该包生成find_package和pkg-config文件，以便于其他包使用此包
cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include/sophus)


