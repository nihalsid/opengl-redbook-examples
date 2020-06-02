cmake_minimum_required(VERSION 2.6)

project(ogltuts)

LINK_DIRECTORIES(${CMAKE_SOURCE_DIR}/lib)

set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

set (CMAKE_DEBUG_POSTFIX "_d")

find_package(OpenGL REQUIRED)

set (COMMON_LIBS ${OPENGL_LIBRARIES} ogl_utilities glfw ${GLFW_LIBRARIES} GL rt dl)

set (COMMON_HEADERS include/LoadShaders.h)

add_library(
    ogl_utilities
    lib/gl3w.c
    lib/LoadShaders.cpp
)

set(RUN_DIR ${PROJECT_SOURCE_DIR}/bin)

set(EXAMPLES
    01-triangle
)

foreach (EXAMPLE ${EXAMPLES}) 
    add_executable(${EXAMPLE} WIN32 src/${EXAMPLE}/${EXAMPLE}.cpp ${COMMON_HEADERS})
    set_property(TARGET ${EXAMPLE} PROPERTY DEBUG_POSTFIX _d)
    target_link_libraries(${EXAMPLE} ${COMMON_LIBS})
endforeach(EXAMPLE)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D_LINUX")

include_directories(include)

ADD_CUSTOM_TARGET(debug ${CMAKE_COMMAND} -DCMAKE_BUILD_TYPE:STRING=Debug ${PROJECT_SOURCE_DIR})
ADD_CUSTOM_TARGET(release ${CMAKE_COMMAND} -DCMAKE_BUILD_TYPE:STRING=Release ${PROJECT_SOURCE_DIR})