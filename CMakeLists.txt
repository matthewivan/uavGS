cmake_minimum_required (VERSION 3.13)
project (uavGS)

set (CMAKE_CXX_STANDARD 17)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++1z -fPIC")

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

find_package(Eigen3 REQUIRED NO_MODULE)
find_package(Boost REQUIRED COMPONENTS system)

find_package(Qt5 #5.6
    COMPONENTS
        Core
        Gui
        OpenGL 
        Svg
        PrintSupport
    REQUIRED)


set(SYSTEM_LIBS pthread
        dl)

if (NOT APPLE)
    set(SYSTEM_LIBS ${SYSTEM_LIBS} rt)
endif(NOT APPLE)

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release")
endif (NOT CMAKE_BUILD_TYPE)

include_directories(${PROJECT_SOURCE_DIR}/include
  ${PROJECT_SOURCE_DIR}/extern
  ${PROJECT_SOURCE_DIR}/extern/QFlightInstruments
  ${Qt5_INCLUDE_DIRS}
  ${Qt5Svg_INCLUDE_DIRS}
  ${Qt5OpenGL_INCLUDE_DIRS})

add_subdirectory(${PROJECT_SOURCE_DIR}/extern/QCustomPlot)
add_subdirectory(${PROJECT_SOURCE_DIR}/extern/QFlightInstruments)

add_subdirectory(src)
#add_subdirectory(tests)

install(DIRECTORY include/uavGS DESTINATION include)
# Not to conflict with uavAP. We should probably do this inside uavAP too
install(DIRECTORY config/ DESTINATION config/uavGS)
install(DIRECTORY resources/ DESTINATION resources/uavGS)