cmake_minimum_required(VERSION 3.0.2)
project(rsu_tracker)

find_package(autoware_build_flags REQUIRED)
find_package(autoware_msgs REQUIRED)
find_package(catkin REQUIRED COMPONENTS
  roscpp
  roslint
  pcl_ros
  geometry_msgs
  tf
  vector_map
  lanelet2_extension
  autoware_msgs
  amathutils_lib
  op_utility
  )


set(CMAKE_CXX_FLAGS "-O2 -Wall ${CMAKE_CXX_FLAGS}")

catkin_package(
  CATKIN_DEPENDS
  roscpp
  pcl_ros
  autoware_msgs
  tf
  vector_map
  amathutils_lib
  op_utility
  )

set(ROSLINT_CPP_OPTS "--filter=-build/c++14")

include_directories(
  ${catkin_INCLUDE_DIRS}
  include)

# rsu_tracker
add_executable(rsu_tracker
        src/main.cpp
        src/imm_ukf_pda.cpp
        src/ukf.cpp
        )
target_link_libraries(rsu_tracker
        ${catkin_LIBRARIES}
        )
add_dependencies(rsu_tracker
        ${catkin_EXPORTED_TARGETS}
        )



install(TARGETS
        rsu_tracker
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
        )

install(DIRECTORY launch/
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
        PATTERN ".svn" EXCLUDE)

if (CATKIN_ENABLE_TESTING)
  roslint_add_test()
endif()