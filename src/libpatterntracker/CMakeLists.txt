# OpenCV
include_directories(${OpenCV_INCLUDE_DIRS})
include_directories( ${CMAKE_SOURCE_DIR}/src/libchessdetector )

if(WIN32)
    # Postfix of .lib and .dll
    set(CMAKE_DEBUG_POSTFIX "d")
endif()

set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)

file (GLOB TRACKER_LIB_SRC "*.cpp")
file (GLOB TRACKER_LIB_HEADER "*.h" "*.hpp")

add_library (patterntracker SHARED ${TRACKER_LIB_SRC} ${TRACKER_LIB_HEADER})

target_link_libraries(patterntracker chessdetector ${OpenCV_LIBS})

install(TARGETS patterntracker
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        )

install(FILES ${TRACKER_LIB_HEADER} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
