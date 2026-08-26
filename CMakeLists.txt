cmake_minimum_required(VERSION 3.22)

project(gamate C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 20)

# Check if we are building for PlayStation 2 via PS2SDK
if(DEFINED ENV{PS2SDK})
    set(BUILD_NAME "gamate.elf")
    message(STATUS "Configuring for PlayStation 2 (PS2SDK)")
else()
    set(BUILD_NAME "${PROJECT_NAME}")
endif()

file(GLOB_RECURSE SRC "src/*.cpp" "src/*.c" "src/*.s" "src/*.S")

add_executable(${BUILD_NAME} ${SRC})
target_include_directories(${BUILD_NAME} PRIVATE src)

if(DEFINED ENV{PS2SDK})
    # PS2 system libraries required for hardware
    target_link_libraries(${BUILD_NAME} -lgraph -ldraw -lpad -laudsrv -lc -lm)
else()
    # Local fallback
    target_link_libraries(${BUILD_NAME} winmm)
endif()