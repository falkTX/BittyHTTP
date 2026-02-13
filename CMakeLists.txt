cmake_minimum_required(VERSION 3.15)
project(BittyHTTP LANGUAGES C)

set(CMAKE_POLICY_DEFAULT_CMP0025 NEW)
set(CMAKE_POLICY_DEFAULT_CMP0063 NEW)
set(CMAKE_POLICY_DEFAULT_CMP0069 NEW)

set(CMAKE_C_STANDARD 11)

set(CMAKE_C_VISIBILITY_PRESET hidden)
set(CMAKE_VISIBILITY_INLINES_HIDDEN TRUE)

set_property(GLOBAL PROPERTY INTERPROCEDURAL_OPTIMIZATION TRUE)

# building interface library
add_library(bittyhttp-interface INTERFACE)
add_library(bittyhttp::interface ALIAS bittyhttp-interface)

target_include_directories(bittyhttp-interface
  INTERFACE
    src
)

target_link_libraries(bittyhttp-interface
  INTERFACE
    $<$<BOOL:${WIN32}>:winmm>
    $<$<BOOL:${WIN32}>:ws2_32>
)

target_sources(bittyhttp-interface
  INTERFACE
    src/WebServer.c
    $<$<NOT:$<BOOL:${WIN32}>>:${PROJECT_SOURCE_DIR}/src/SocketsCon.c>
    $<$<BOOL:${WIN32}>:${PROJECT_SOURCE_DIR}/src/SocketsConWin.c>
)

#######################################################################################################################
