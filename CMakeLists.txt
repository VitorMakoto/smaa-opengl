#cmake ver req
cmake_minimum_required(VERSION 2.6)

set(project_name smaa)

#project name
project(${project_name})

#so that it talks to us
set(CMAKE_VERBOSE_MAKEFILE ON)
set(CMAKE_ALLOW_LOOSE_LOOPS TRUE)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")

if(UNIX)
#some compiler directives to make sure all warnings etc. are caught, 
#and those that come from 3rd party libs are not
add_definitions( "-Wall -Wno-long-long -ansi -pedantic" )
endif()

#header files source
include_directories(${CMAKE_SOURCE_DIR}/src/)
link_directories(${CMAKE_SOURCE_DIR}/src/)

#put depencencies here
include_directories(${CMAKE_SOURCE_DIR}/external/include/)

find_package(GLEW REQUIRED)
if (GLEW_FOUND)
   include_directories(${GLEW_INCLUDE_DIRS})
   link_libraries(${GLEW_LIBRARIES})
MESSAGE(STATUS "CMAKE_LIBRARY:" ${GLEW_LIBRARIES})

endif()

find_package(SFML 2 COMPONENTS window system audio)
include_directories(${SFML_INCLUDE_DIR})
link_libraries(${SFML_LIBRARIES})

MESSAGE(STATUS "CMAKE_LIBRARY:" ${SFML_LIBRARIES})

find_package(OpenGL REQUIRED)
include_directories(${OpenGL_INCLUDE_DIRS})
link_directories(${OpenGL_LIBRARY_DIRS})
add_definitions(${OpenGL_DEFINITIONS})
link_libraries(${OPENGL_LIBRARIES})

find_package(PkgConfig REQUIRED)
pkg_search_module(GLFW REQUIRED glfw3)
include_directories(${GLFW_INCLUDE_DIRS})
link_libraries(${GLFW_LIBRARIES})
link_directories(${GLFW_LIBRARY_DIRS})

link_directories(${CMAKE_SOURCE_DIR}/external/lib/)

#adding the project's exe
add_executable(${project_name} main)

target_link_libraries(${project_name} ${${project_name}_external_libs})