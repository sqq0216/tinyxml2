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

option(BUILD_SHARED_LIBS "build as shared library" ON)
option(BUILD_STATIC_LIBS "build as static library" OFF)

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

#add_executable(xmltest xmltest.cpp)
#if(BUILD_SHARED_LIBS)
#   add_dependencies(xmltest tinyxml2)
#   target_link_libraries(xmltest tinyxml2)
#else(BUILD_STATIC_LIBS)
#   add_dependencies(xmltest tinyxml2_static)
#   target_link_libraries(xmltest tinyxml2_static)
#endif()
#
## Copy test resources and create test output directory
#add_custom_command(TARGET xmltest POST_BUILD
#  COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_SOURCE_DIR}/resources $<TARGET_FILE_DIR:xmltest>/resources
#  COMMAND ${CMAKE_COMMAND} -E make_directory $<TARGET_FILE_DIR:xmltest>/resources/out
#  COMMENT "Configuring xmltest resources directory: ${CMAKE_BINARY_DIR}/resources"
#)
#
#install(FILES src/tinyxml2.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
#
#foreach(p LIB INCLUDE)
#	set(var CMAKE_INSTALL_${p}DIR)
#	if(NOT IS_ABSOLUTE "${${var}}")
#		set(${var} "${CMAKE_INSTALL_PREFIX}/${${var}}")
#	endif()
#endforeach()
#
#configure_file(tinyxml2.pc.in tinyxml2.pc @ONLY)
#install(FILES ${CMAKE_CURRENT_BINARY_DIR}/tinyxml2.pc DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)
#
##add_test(xmltest ${SAMPLE_NAME} COMMAND $<TARGET_FILE:${SAMPLE_NAME}>)
#
## uninstall target
#configure_file(
#    "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
#    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
#    IMMEDIATE @ONLY)
#
#add_custom_target(uninstall
#    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)

