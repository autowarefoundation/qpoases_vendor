cmake_minimum_required(VERSION 3.5)

project(qpoases_vendor)

set(PACKAGE_VERSION 3.2.0)

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

configure_file(qpoases_vendorConfig.cmake.in "${PROJECT_BINARY_DIR}/qpoases_vendorConfig.cmake" @ONLY)
configure_file(qpoases_vendorConfig-version.cmake.in "${PROJECT_BINARY_DIR}/qpoases_vendorConfig-version.cmake" @ONLY)

install(FILES
  "${PROJECT_BINARY_DIR}/qpoases_vendorConfig.cmake"
  "${PROJECT_BINARY_DIR}/qpoases_vendorConfig-version.cmake"
  DESTINATION share/${PROJECT_NAME}/cmake
  )
