cmake_minimum_required(VERSION 3.6.0)
project(Tutorial)

# The version number
set(${PROJECT_NAME}_VERSION_MAJOR 0)
set(${PROJECT_NAME}_VERSION_MINOR 1)

# configure a header file to pass some of the CMake settings
configure_file(
  "${PROJECT_SOURCE_DIR}/TutorialConfig.h.in"
  "${PROJECT_BINARY_DIR}/TutorialConfig.h"
)

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
include_directories("${PROJECT_BINARY_DIR}")


# should we use our own math functions?
option (USE_MYMATH 
  "Use tutorial provided math implementation" ON)

ADD_DEFINITIONS(-DUSE_MYMATH)
# add the MathFunctions library?
#
if(USE_MYMATH)
  set(MYMATH_DIR "../MathFunctions")
  include_directories(${MYMATH_DIR})
  set(EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
endif(USE_MYMATH)


# add the executable
add_executable(${PROJECT_NAME} Tutorial.cc)
target_link_libraries(${PROJECT_NAME} ${EXTRA_LIBS})


# add the install targets (存在预先定义好的 CMAKE_INSTALL_PREFIX)
# RUNTIME LIBRARY ARCHIVE
install (TARGETS ${PROJECT_NAME} DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h" DESTINATION include)
