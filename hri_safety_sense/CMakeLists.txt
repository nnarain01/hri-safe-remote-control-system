cmake_minimum_required(VERSION 2.8.3)
#include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
project(hri_safety_sense)
# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

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

install(TARGETS ${PROJECT_NAME}_node ${PROJECT_NAME}_driver
  DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)
install(DIRECTORY launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(terex ${PROJECT_NAME})
#rosbuild_add_executable(talker src/talker.cpp)
#rosbuild_add_executable(listener src/listener.cpp)
#target_link_libraries(terex hri_common)

#rosbuild_add_executable(safe_remote_control src/main.cpp src/VscProcess.cpp src/VehicleInterface.c src/JoystickHandler.cpp src/SerialInterface.c)
