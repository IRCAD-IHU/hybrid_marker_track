cmake_minimum_required(VERSION 3.1)

include(GNUInstallDirs)
project(hybrid_marker_track)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib )
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib )

# On Windows, if the user doesn't specify a value,
# 'CMAKE_BUILD_TYPE' is automatically initialized to 'Debug' after 'project()'.
# So we need to check this variable at this point.
set(CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE} CACHE STRING "Choose the type of build, options are: Debug, Release, RelWithDebInfo or MinSizeRel")
set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug;Release;RelWithDebInfo;MinSizeRel")

if(NOT CMAKE_BUILD_TYPE STREQUAL "Debug"
        AND NOT CMAKE_BUILD_TYPE STREQUAL "Release"
        AND NOT CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo"
        AND NOT CMAKE_BUILD_TYPE STREQUAL "MinSizeRel")
    message(FATAL_ERROR "Invalid value for CMAKE_BUILD_TYPE: '${CMAKE_BUILD_TYPE}' (required Debug, Release, RelWithDebInfo or MinSizeRel)")
endif()

set(OpenCV_STATIC OFF)

# Find OpenCV
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

#Find IPPE
find_package(IPPE REQUIRED)
include_directories(${IPPE_INCLUDE_DIRS})

# Pattern tracker
add_subdirectory(src/libpatterntracker)
add_subdirectory(src/libchessdetector)

include_directories(
	${CMAKE_CURRENT_BINARY_DIR}
	${CMAKE_CURRENT_SOURCE_DIR}/include
	${CMAKE_CURRENT_SOURCE_DIR}/src/libpatterntracker
	${CMAKE_CURRENT_SOURCE_DIR}/src/libchessdetector
)

file(GLOB HEADER_FILES include/*.h)
file(GLOB CXX_FILES src/*.cpp)

add_executable(
	hybrid_marker_track
	${CXX_FILES}
	${HEADER_FILES}
)

target_link_libraries(
	hybrid_marker_track
	patterntracker
	chessdetector
	${IPPE_LIBRARIES}
	${OpenCV_LIBS}
)