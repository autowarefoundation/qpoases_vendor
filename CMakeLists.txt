cmake_minimum_required(VERSION 3.5)

project(qpoases_vendor)

find_package(catkin REQUIRED)

catkin_package(
)

include(ExternalProject)
ExternalProject_Add(qpoases_3_2
  SVN_REPOSITORY https://projects.coin-or.org/svn/qpOASES/stable/3.2
  CONFIGURE_COMMAND ""
  BUILD_IN_SOURCE TRUE
  INSTALL_COMMAND ""
)

ExternalProject_Get_property(qpoases_3_2 SOURCE_DIR)
install(
  DIRECTORY ${SOURCE_DIR}/include/qpOASES
  DESTINATION include
)
install(
  FILES ${SOURCE_DIR}/include/qpOASES.hpp
  DESTINATION include
)
install(
  FILES ${SOURCE_DIR}/bin/libqpOASES.so
  DESTINATION lib
)
