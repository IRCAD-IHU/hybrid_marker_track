# OpenCV
include_directories(${OpenCV_INCLUDE_DIRS})

file(GLOB cpp "*.cpp")
file(GLOB head "*.h")

if(WIN32)
    # Postfix of .lib and .dll
    set(CMAKE_DEBUG_POSTFIX "d")
endif()

set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)

add_library(chessdetector SHARED ${cpp} ${head})
target_link_libraries(chessdetector PUBLIC ${OpenCV_LIBS})

find_package(OpenMP)
if(${OPENMP_FOUND})
    if(UNIX)
        target_link_libraries(chessdetector PRIVATE ${OpenMP_CXX_FLAGS})
    endif(UNIX)
    target_compile_options(chessdetector PRIVATE ${OpenMP_CXX_FLAGS})
else()
    message(INFO "OpenMP not found: parallel optimization is disabled")
endif()

install(TARGETS chessdetector
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        )

install(FILES ${head} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
