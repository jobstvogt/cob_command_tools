cmake_minimum_required(VERSION 2.8.3)
project(cob_teleop)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
	roscpp
	dynamic_reconfigure
	actionlib
	sensor_msgs
	geometry_msgs
	std_msgs
	std_srvs
	trajectory_msgs
	cob_script_server
)

# protected region additional user defined REQUIREMENTS on begin #
# protected region additional user defined REQUIREMENTS end #

generate_dynamic_reconfigure_options(cfg/cob_teleop_v2.cfg )

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
  INCLUDE_DIRS
	common/include
  CATKIN_DEPENDS 
  	roscpp
    dynamic_reconfigure
	sensor_msgs
	geometry_msgs
	std_msgs
	std_srvs
	trajectory_msgs
	cob_script_server
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(common/include)
include_directories(${catkin_INCLUDE_DIRS})


## Declare a cpp executable
add_executable(cob_teleop_v2 ros/src/cob_teleop_v2_ros.cpp)

## Add cmake target dependencies of the executable/library
## as an example, message headers may need to be generated before nodes
add_dependencies(cob_teleop_v2 cob_teleop_v2_gencfg)
add_dependencies(cob_teleop_v2 ${cob_teleop_EXPORTED_TARGETS})

## Specify libraries to link a library or executable target against
target_link_libraries(cob_teleop_v2
  ${catkin_LIBRARIES}
)

# protected region additional user defined BUILD STATEMENTS on begin #
add_executable(cob_teleop_v1 ros/src/cob_teleop_v1.cpp)
add_dependencies(cob_teleop_v1 std_srvs_gencpp)
add_executable(cob_teleop_keyboard ros/src/cob_teleop_keyboard.cpp)
add_executable(keyboard_publisher ros/src/keyboard_publisher.cpp)

target_link_libraries(cob_teleop_v1 ${catkin_LIBRARIES})
target_link_libraries(cob_teleop_keyboard ${catkin_LIBRARIES})
target_link_libraries(keyboard_publisher ${catkin_LIBRARIES})
# protected region additional user defined BUILD STATEMENTS end #


#############
## Install ##
#############

## Mark executables and/or libraries for installation
install(TARGETS cob_teleop_v2
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

# protected region user Cmake macros on begin #
install(TARGETS cob_teleop_v1
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(TARGETS cob_teleop_keyboard
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(TARGETS keyboard_publisher 
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

## Mark cpp header files for installation
install(DIRECTORY common/include/${PROJECT_NAME}/
 DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
 FILES_MATCHING PATTERN "*.h"
 PATTERN ".svn" EXCLUDE
)

install(DIRECTORY ros/launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
# protected region user Cmake macros end #
