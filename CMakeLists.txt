cmake_minimum_required(VERSION 3.5)

project(qpoases_vendor)

find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()

include(ExternalProject)
ExternalProject_Add(qpoases_3_2
  SVN_REPOSITORY https://projects.coin-or.org/svn/qpOASES/stable/3.2
  SVN_TRUST_CERT TRUE
  BUILD_IN_SOURCE TRUE
  INSTALL_COMMAND ""
)

ExternalProject_Get_property(qpoases_3_2 SOURCE_DIR)

set(qpOASES_CXX_LIBRARY ${SOURCE_DIR}/libs/libqpOASES.a)

add_library(qpOASES STATIC IMPORTED
  IMPORTED_LOCATION ${qpOASES_CXX_LIBRARY})

install(DIRECTORY ${SOURCE_DIR}/include/
  DESTINATION include
)

install(FILES ${qpOASES_CXX_LIBRARY}
  DESTINATION lib
)

ament_export_libraries(${qpOASES})

ament_auto_package()
