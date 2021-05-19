cmake_minimum_required(VERSION 3.0.2)
project(fs_navigation)

find_package(catkin REQUIRED
COMPONENTS laser_assembler roscpp sensor_msgs)

catkin_package()

include_directories(
  ${catkin_INCLUDE_DIRS}
)

add_executable(laser_convert2_cloud_node src/laser_convert2_cloud_node.cpp)

add_dependencies(laser_convert2_cloud_node ${catkin_EXPORTED_TARGETS})

target_link_libraries(laser_convert2_cloud_node
  ${catkin_LIBRARIES}
)

install(
  TARGETS laser_convert2_cloud_node
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(
  DIRECTORY config launch meshes urdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

# decompression step to unzip only_ravenport_town_scaled_100_reduced.tar.xz
add_custom_target(mesh ALL)
add_custom_command(
  TARGET
    mesh
  PRE_BUILD
  COMMAND
    ${CMAKE_COMMAND} -E tar xzf ${CMAKE_SOURCE_DIR}/meshes/only_ravenport_town_scaled_100_reduced.tar.xz
  DEPENDS
    ${CMAKE_SOURCE_DIR}/meshes/only_ravenport_town_scaled_100_reduced.tar.xz
  WORKING_DIRECTORY
    ${CMAKE_BINARY_DIR}
  VERBATIM
)

install(
  FILES ${CMAKE_BINARY_DIR}/only_ravenport_town_scaled_100_reduced.stl
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/meshes
)
