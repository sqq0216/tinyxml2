cmake_minimum_required(VERSION 3.5)
project(tinyxml2)

include_directories(${PROJECT_SOURCE_DIR}/src)

set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -DDEBUG")

include_directories(headers)

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/output)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/output)


################################
# Add targets
# By Default shared libray is being built
# To build static libs also - Do cmake . -DBUILD_STATIC_LIBS:BOOL=ON
# User can choose not to build shared library by using cmake -DBUILD_SHARED_LIBS:BOOL=OFF
# To build only static libs use cmake . -DBUILD_SHARED_LIBS:BOOL=OFF -DBUILD_STATIC_LIBS:BOOL=ON

option(BUILD_SHARED_LIBS "build as shared library" OFF)
option(BUILD_STATIC_LIBS "build as static library" ON)

if(BUILD_SHARED_LIBS)
    add_library(tinyxml2 SHARED src/tinyxml2.cpp)

set_target_properties(tinyxml2 PROPERTIES
        COMPILE_DEFINITIONS "TINYXML2_EXPORT"
	VERSION "${GENERIC_LIB_VERSION}"
	SOVERSION "${GENERIC_LIB_SOVERSION}")

endif()

if(BUILD_STATIC_LIBS)
    add_library(tinyxml2_static STATIC src/tinyxml2.cpp src/tinyxml2.h)

    set_target_properties(tinyxml2_static PROPERTIES
        COMPILE_DEFINITONS "TINYXML2_EXPORT"
        VERSION "${GENERIC_LIB_VERSION}"
        SOVERSION "${GENERIC_LIB_SOVERSION}")
set_target_properties( tinyxml2_static PROPERTIES OUTPUT_NAME tinyxml2 )

endif()