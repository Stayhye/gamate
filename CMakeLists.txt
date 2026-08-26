cmake_minimum_required(VERSION 3.22)

project(gamate C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 20)

# Check if we are cross-compiling for PS2
if(DEFINED ENV{PS2SDK})
    set(BUILD_NAME "gamate.elf")
    message(STATUS "Configuring for PlayStation 2 (PS2SDK)")
else()
    set(BUILD_NAME "${PROJECT_NAME}")
    set(OUTPUT_DIR "${CMAKE_SOURCE_DIR}/bin/${CMAKE_BUILD_TYPE}")
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${OUTPUT_DIR}")
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${OUTPUT_DIR}")
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${OUTPUT_DIR}")
endif()

file(GLOB_RECURSE SRC "src/*.cpp" "src/*.c" "src/*.s" "src/*.S")

add_executable(${BUILD_NAME} ${SRC})
target_include_directories(${BUILD_NAME} PRIVATE src)

# Platform-specific dependencies
if(DEFINED ENV{PS2SDK})
    # PS2 Libraries (Graphics, pads, audio)
    target_link_libraries(${BUILD_NAME} -lgraph -ldraw -lpad -laudsrv -lc -lm)
else()
    # Windows native
    target_link_libraries(${BUILD_NAME} winmm)
endif()