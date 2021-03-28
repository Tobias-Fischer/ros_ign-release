cmake_minimum_required(VERSION 3.5)

project(ros_ign_gazebo)

find_package(catkin
  REQUIRED
  COMPONENTS
    roscpp
)

find_package(ignition-transport9 REQUIRED)
set(IGN_TRANSPORT_VER ${ignition-transport9_VERSION_MAJOR})

find_package(ignition-msgs6 REQUIRED)
set(IGN_MSGS_VER ${ignition-msgs6_VERSION_MAJOR})

find_package(ignition-gazebo4 REQUIRED)
set(IGN_GAZEBO_VER ${ignition-gazebo4_VERSION_MAJOR})

message(STATUS "Compiling against Ignition Dome")

ign_find_package(gflags
    REQUIRED
    PKGCONFIG gflags)

catkin_package()


include_directories(include ${catkin_INCLUDE_DIRS})

set(cpp_execs
  create
)

foreach(exec ${cpp_execs})
  add_executable(${exec} src/${exec}.cpp)
  target_link_libraries(${exec}
    ${catkin_LIBRARIES}
    gflags
    ignition-msgs${IGN_MSGS_VER}::core
    ignition-transport${IGN_TRANSPORT_VER}::core
  )
  install(TARGETS ${exec}
    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  )
endforeach()

install(
  PROGRAMS
    scripts/ign_gazebo
  DESTINATION
    ${CATKIN_PACKAGE_BIN_DESTINATION})

install(
  DIRECTORY
    launch/
  DESTINATION
    ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
)

