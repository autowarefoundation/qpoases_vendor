cmake_minimum_required(VERSION 3.5)

project(qpoases_vendor)

find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()


include(ExternalProject)
ExternalProject_Add(qpoases_3_2
  GIT_REPOSITORY https://github.com/coin-or/qpOASES
  GIT_TAG stable/3.2
  SVN_TRUST_CERT TRUE
  BUILD_IN_SOURCE TRUE
  CMAKE_ARGS "-DCMAKE_POSITION_INDEPENDENT_CODE=ON"
  INSTALL_COMMAND ""
)

ExternalProject_Get_property(qpoases_3_2 SOURCE_DIR)

set(qpOASES_CXX_LIBRARY ${SOURCE_DIR}/libs/libqpOASES.a)

install(DIRECTORY ${SOURCE_DIR}/include/
  DESTINATION include
)

install(FILES ${qpOASES_CXX_LIBRARY}
  DESTINATION lib
)

ament_export_libraries(libqpOASES.a)

ament_auto_package()
