# Do not modify this file unless absolutely necessary.
# All project-specific settings can be found in the file:
# project_config.cmake

cmake_minimum_required(VERSION 3.10)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

project(Celeste C CXX)

set(EXPORT_DIR ${CMAKE_CURRENT_SOURCE_DIR}/export)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${EXPORT_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${EXPORT_DIR})

include(project_config)
include(get_SDL2)

get_SDL2("2.26.0")
get_SDL2_mixer("2.6.2")
getSDL2_image("2.")
endif()

set(GUI_TYPE "")

if (CMAKE_BUILD_TYPE MATCHES Release)
  #-- Configure the OS X Bundle Plist
  if (APPLE)
    set(GUI_TYPE MACOSX_BUNDLE)
  elseif(WIN32)
    set(GUI_TYPE WIN32)
  endif()
endif()

add_executable(${PROJECT_TITLE} ${GUI_TYPE} ${project_sources})

set_target_properties(${PROJECT_TITLE} PROPERTIES C_STANDARD 90 CXX_STANDARD 11)

target_include_directories(
  ${PROJECT_TITLE}
  PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src
  PRIVATE ${SDL2_INCLUDE_DIRS})

if (PS2)
  set_target_properties(${PROJECT_TITLE} PROPERTIES LINKER_LANGUAGE CXX)#
    set(SDL2_MIXER_LIBS
    SDL2_mixer
    xmp
    modplug
    )
endif()

add_compile_definitions(
  ${PROJECT_TITLE}
  _CRT_SECURE_NO_WARNINGS)

target_link_libraries(${PROJECT_TITLE} PRIVATE ${SDL2_MIXER_LIBS})
target_link_libraries(${PROJECT_TITLE} PRIVATE ${SDL2_LIBRARIES})

if(CMAKE_C_COMPILER_ID STREQUAL "GNU" AND UNIX)
  target_link_libraries(${PROJECT_TITLE} PRIVATE m)
endif()

if (CMAKE_C_COMPILER_ID STREQUAL "Clang")
  set(COMPILE_OPTIONS
    -Wall
    -Wextra
    -Wpedantic)

elseif (CMAKE_C_COMPILER_ID STREQUAL "GNU")
  set(COMPILE_OPTIONS
    -Wall
    -Wextra
    -Wpedantic)

elseif (CMAKE_C_COMPILER_ID STREQUAL "MSVC")
  set(COMPILE_OPTIONS
    /W4
    /utf-8)
endif()
