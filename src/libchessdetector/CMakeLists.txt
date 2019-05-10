# OpenCV
include_directories(${OpenCV_INCLUDE_DIRS})

file(GLOB cpp "*.cpp")
file(GLOB head "*.h")


if(WIN32)
    # Postfix of .lib and .dll
    set(CMAKE_DEBUG_POSTFIX "d")
endif()

add_library(chessdetector SHARED ${cpp} ${head})

install(TARGETS chessdetector
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        )

install(FILES ${head} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})