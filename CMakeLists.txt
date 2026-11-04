cmake_minimum_required(VERSION 3.21)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
if ("${CMAKE_SYSTEM_NAME}" STREQUAL "iOS" OR IOS)
  set(CMAKE_OSX_ARCHITECTURES "arm64")
else()
    set(CMAKE_OSX_ARCHITECTURES "arm64;x86_64")
endif()
set(CMAKE_CXX_VISIBILITY_PRESET hidden)

project(HappyTextures VERSION 1.0.0)

file(GLOB_RECURSE SOURCES CONFIGURE_DEPENDS src/*.cpp)

add_library(${PROJECT_NAME} SHARED ${SOURCES})

if (NOT DEFINED ENV{GEODE_SDK})
    message(FATAL_ERROR "Unable to find Geode SDK! Please define GEODE_SDK environment variable to point to Geode")
else()
    message(STATUS "Found Geode: $ENV{GEODE_SDK}")
endif()

add_subdirectory($ENV{GEODE_SDK} ${CMAKE_CURRENT_BINARY_DIR}/geode)

CPMAddPackage(
    NAME AlphasGeodeUtils
    GITHUB_REPOSITORY scratchcatnuvola/Alphas-Geode-Utils
    GIT_TAG main
)

CPMAddPackage(
    NAME TextureLoader
    GITHUB_REPOSITORY scratchcatnuvola/textureldr
    GIT_TAG main
)

CPMAddPackage(
    NAME NodeIDs
    GITHUB_REPOSITORY scratchcatnuvola/NodeIDs
    GIT_TAG main
)

CPMAddPackage(
    NAME ImagePlus
    GITHUB_REPOSITORY scratchcatnuvola/ImagePlus
    GIT_TAG main
)
include(${CMAKE_SOURCE_DIR}/cmake/IncludeLibs.cmake)
target_link_libraries(${PROJECT_NAME} third_party)

setup_geode_mod(${PROJECT_NAME})
