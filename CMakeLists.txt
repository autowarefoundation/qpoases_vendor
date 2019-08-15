cmake_minimum_required(VERSION 3.5)

project(qpoases_vendor)

find_package(catkin REQUIRED)

include(ExternalProject)
ExternalProject_Add(qpoases_3_2
  SVN_REPOSITORY https://projects.coin-or.org/svn/qpOASES/stable/3.2
  SVN_TRUST_CERT TRUE
  CONFIGURE_COMMAND ""
  BUILD_IN_SOURCE TRUE
  INSTALL_COMMAND ""
)

ExternalProject_Get_property(qpoases_3_2 SOURCE_DIR)

set(qpOASES_CXX_LIBRARY ${SOURCE_DIR}/bin/libqpOASES.so)

add_custom_target(create_include_dir ALL
  COMMAND ${CMAKE_COMMAND} -E make_directory ${CATKIN_GLOBAL_INCLUDE_DESTINATION}/${PROJECT_NAME}
  DEPENDS qpoases_3_2
)

# "We do not recommend using GLOB to collect a list of source files from your source tree."
set(qpOASES_SRC
  ${SOURCE_DIR}/src/BLASReplacement.cpp
  ${SOURCE_DIR}/src/Bounds.cpp
  ${SOURCE_DIR}/src/Constraints.cpp
  ${SOURCE_DIR}/src/Flipper.cpp
  ${SOURCE_DIR}/src/Indexlist.cpp
  ${SOURCE_DIR}/src/LAPACKReplacement.cpp
  ${SOURCE_DIR}/src/Matrices.cpp
  ${SOURCE_DIR}/src/MessageHandling.cpp
  ${SOURCE_DIR}/src/Options.cpp
  ${SOURCE_DIR}/src/OQPinterface.cpp
  ${SOURCE_DIR}/src/QProblemB.cpp
  ${SOURCE_DIR}/src/QProblem.cpp
  ${SOURCE_DIR}/src/SolutionAnalysis.cpp
  ${SOURCE_DIR}/src/SQProblem.cpp
  ${SOURCE_DIR}/src/SubjectTo.cpp
  ${SOURCE_DIR}/src/Utils.cpp
)

set(qpOASES_PYTHON_SRC
  ${SOURCE_DIR}/interfaces/python/qpoases.pxd
  ${SOURCE_DIR}/interfaces/python/qpoases.pyx
)

# the files listed above are present when qpoases_3_2 (i.e. getting source files) is completed
add_custom_command(OUTPUT ${qpOASES_SRC} ${qpOASES_PYTHON_SRC}
  DEPENDS qpoases_3_2
)

# Custom build Python wrappers
add_custom_target(qpOASES_Python ALL
  COMMAND python setup.py build_ext --include-dirs ${SOURCE_DIR}/include ${INCLUDE_DIRECTORIES} --build-lib ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_PYTHON_DESTINATION} --build-temp ${CMAKE_CURRENT_BINARY_DIR} --sources ${qpOASES_PYTHON_SRC} ${qpOASES_SRC}
  WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
  DEPENDS qpoases_3_2
  COMMENT "Building Python wrappers."
)

# Tell CMake about the build output
set(qpOASES_PYTHON_LIB
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_PYTHON_DESTINATION}/libqpOASES_Python.so
)

add_custom_command(OUTPUT ${qpOASES_PYTHON_LIB}
  DEPENDS qpOASES_Python
)

catkin_package(
  INCLUDE_DIRS ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  LIBRARIES qpOASES qpOASES_Python
)

install(DIRECTORY ${SOURCE_DIR}/include/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

install(FILES ${qpOASES_PYTHON_LIB}
  DESTINATION ${CATKIN_GLOBAL_PYTHON_DESTINATION}
)

install(FILES ${qpOASES_CXX_LIBRARY}
  DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)
