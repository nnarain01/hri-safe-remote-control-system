cmake_minimum_required(VERSION 2.8.3)

project(hri_safety_sense)

#rosbuild_init()
find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  sensor_msgs
  message_generation
)

add_service_files(
  FILES
    EmergencyStop.srv
    KeyString.srv
    KeyValue.srv
)
generate_messages(
  DEPENDENCIES
    std_msgs
)

catkin_package()


include_directories(
  ../drivers/C/include/
  ${catkin_INCLUDE_DIRS}
)

# add vcs driver
add_library(${PROJECT_NAME}_driver
  ../drivers/C/src/SerialInterface.c
  ../drivers/C/src/VehicleInterface.c
)
set_target_properties(${PROJECT_NAME}_driver PROPERTIES
  COMPILE_FLAGS "-fPIC -g -Wall -Wextra"
)

# add ROS node
add_executable(${PROJECT_NAME}_node
  src/main.cpp
  src/JoystickHandler.cpp
  src/VscProcess.cpp
)
target_link_libraries(${PROJECT_NAME}_node
  ${PROJECT_NAME}_driver
  ${catkin_LIBRARIES}
)

add_dependencies(${PROJECT_NAME}_node
  hri_safety_sense_generate_messages_cpp
)

install(TARGETS ${PROJECT_NAME}_node ${PROJECT_NAME}_driver
  DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)
install(DIRECTORY launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
