cmake_minimum_required(VERSION 2.8.3)
project(libsbp_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(libsbp
  GIT_REPOSITORY git@github.com:swift-nav/libsbp.git
  GIT_TAG v2.6.5
  UPDATE_COMMAND git submodule update --init
  CMAKE_ARGS
    -DBUILD_SHARED_LIBS=ON
    -CMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}
  CONFIGURE_COMMAND cmake ../libsbp/c/
  BUILD_COMMAND make
  INSTALL_COMMAND ""
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} libsbp)
#target_link_libraries(${PROJECT_NAME} ${CATKIN_DEVEL_PREFIX}/lib/liblibsbp${CMAKE_SHARED_LIBRARY_SUFFIX})

cs_install()

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
          CFG_EXTRAS libsbp-extras.cmake)
