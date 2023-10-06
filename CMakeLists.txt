cmake_minimum_required(VERSION 3.5)
project(rosbridge_client)

find_package(catkin REQUIRED)

catkin_package(
        CATKIN_DEPENDS rospy roslibpy  # rospy_message_converter
)

install(PROGRAMS
        src/rosbridge_client/rosbridge_transceiver
        DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
        )
