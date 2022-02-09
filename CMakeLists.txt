cmake_minimum_required(VERSION 2.8.3)
project(steering_functions)


## Compile as C++11
add_compile_options(-std=c++11)


find_package(Eigen3 REQUIRED)


###########
## Build ##
###########

include_directories(
  include
  ${EIGEN3_INCLUDE_DIRS}
)

## Get all source files
file(GLOB SRC_HC_CC_SS src/hc_cc_state_space/*.cpp)
set(SRC_FILES
    src/dubins_state_space/dubins_state_space.cpp
    src/reeds_shepp_state_space/reeds_shepp_state_space.cpp
    src/utilities/utilities.cpp
    ${SRC_HC_CC_SS})

## Declare a C++ library
add_library(${PROJECT_NAME} ${SRC_FILES})
#add_executable(${PROJECT_NAME}_node src/${PROJECT_NAME}_node.cpp ${SRC_FILES})

set_target_properties(${PROJECT_NAME}
                      PROPERTIES POSITION_INDEPENDENT_CODE ON)

target_link_libraries(${PROJECT_NAME})

set(${PROJECT_NAME}_INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/include
                                 ${EIGEN3_INCLUDE_DIRS}
    CACHE INTERNAL "${PROJECT_NAME}: Include Directories" FORCE)

install(TARGETS ${PROJECT_NAME}
  EXPORT ${PROJECT_NAME}-targets
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}/steering_functions"
)


