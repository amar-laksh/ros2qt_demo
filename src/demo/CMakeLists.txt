cmake_minimum_required(VERSION 3.5)
project(demo)

set (CMAKE_CXX_STANDARD 14)
if(NOT WIN32)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -Wall -Wextra -fPIC")
endif()

IF (NOT DEFINED BUILD_VERSION)
	SET(BUILD_VERSION "not set")
ENDIF()
ADD_DEFINITIONS(-DBUILD_VERSION="${BUILD_VERSION}")

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rmw_implementation REQUIRED)
find_package(std_msgs REQUIRED)
find_package(Qt5Core REQUIRED)
find_package(Qt5Quick REQUIRED)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)


include_directories(
	${rclcpp_INCLUDE_DIRS}
	${std_msgs_INCLUDE_DIRS}
	${Qt5Core_INCLUDE_DIRS}
	${Qt5Quick_INCLUDE_DIRS}
	)

file(GLOB SOURCE_FILES
	"src/*.cpp"
	)

add_executable(${PROJECT_NAME} ${SOURCE_FILES} "src/qml.qrc")

ament_target_dependencies(${PROJECT_NAME}
	rclcpp
	std_msgs
	rmw_implementation
	)
target_link_libraries(${PROJECT_NAME}
	Qt5::Core
	Qt5::Quick
	)

install(TARGETS ${PROJECT_NAME} DESTINATION bin)

ament_package()
