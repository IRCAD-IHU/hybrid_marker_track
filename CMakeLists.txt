cmake_minimum_required(VERSION 2.8.11)

#if(POLICY CMP0020)
#	cmake_policy(SET CMP0020 NEW)
#endif()
include(GNUInstallDirs)
project(hybrid_marker_track)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib )
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib )

# set(CMAKE_LIBRARY_PATH "C:\\Program Files (x86)\\Windows Kits\\8.0\\Lib\\win8\\um\\x64")

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
find_package( OpenCV REQUIRED )
include_directories(${OpenCV_INCLUDE_DIRS})

#Find IPPE
find_package(Ippe REQUIRED)
include_directories(${IPPE_INCLUDE_DIRS})

# find_package(Qt5Widgets REQUIRED)
# set(CMAKE_AUTOMOC ON)


# IF(WIN32)
#     set(PTHREADS_INCLUDE_DIR "" CACHE PATH "Pthreads Include Directory")
#   set(PTHREADS_LIB_DIR "" CACHE PATH "Pthreads Lib Directory")
# ELSEIF(UNIX)
#   find_package(Threads)
# ENDIF()

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

add_executable(hybrid_marker_track
		${CXX_FILES}
		${HEADER_FILES}
		)

target_link_libraries(hybrid_marker_track
		${OpenCV_LIBS}
		${IPPE_LIBRARIES}
		patterntracker
		chessdetector
		)
