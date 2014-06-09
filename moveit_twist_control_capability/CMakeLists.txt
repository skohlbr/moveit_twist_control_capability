cmake_minimum_required(VERSION 2.8.3)
project(moveit_twist_control_capability)

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

find_package(Boost REQUIRED system filesystem date_time program_options signals thread)
find_package(catkin REQUIRED COMPONENTS
  moveit_core
  moveit_ros_planning
  actionlib
  roscpp
  pluginlib
  tf
  moveit_ros_move_group
)

catkin_package(
  LIBRARIES
    moveit_move_group_capabilities_base
  INCLUDE_DIRS
    include
  CATKIN_DEPENDS
    moveit_core
    moveit_ros_planning
)

include_directories(include)
include_directories(${catkin_INCLUDE_DIRS})
include_directories(SYSTEM
                    ${Boost_INCLUDE_DIRS}
                    )

link_directories(${Boost_LIBRARY_DIRS})
link_directories(${catkin_LIBRARY_DIRS})

#add_library(moveit_move_group_capabilities_base
#  src/move_group_context.cpp
#  src/move_group_capability.cpp
#  )
#add_dependencies(moveit_move_group_capabilities_base ${catkin_EXPORTED_TARGETS}) # wait until all *_msgs packages are finished being built

#add_executable(move_group src/move_group.cpp)

#add_executable(list_move_group_capabilities src/list_capabilities.cpp)

add_library(moveit_twist_control_capability
  src/twist_control_capability.cpp
  )
target_link_libraries(moveit_twist_control_capability ${catkin_LIBRARIES} ${Boost_LIBRARIES})

#target_link_libraries(moveit_move_group_capabilities_base ${catkin_LIBRARIES} ${Boost_LIBRARIES})
#target_link_libraries(move_group moveit_move_group_capabilities_base ${catkin_LIBRARIES} ${Boost_LIBRARIES})
#target_link_libraries(moveit_move_group_default_capabilities moveit_move_group_capabilities_base ${catkin_LIBRARIES} ${Boost_LIBRARIES})
#target_link_libraries(list_move_group_capabilities ${catkin_LIBRARIES} ${Boost_LIBRARIES})

install(TARGETS moveit_twist_control_capability
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

install(DIRECTORY include/ DESTINATION include)

#install(PROGRAMS
#  scripts/load_map scripts/save_map DESTINATION
#  ${CATKIN_PACKAGE_BIN_DESTINATION})

install(FILES
  twist_control_capability_plugin_description.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})