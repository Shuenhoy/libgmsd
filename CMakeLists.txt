cmake_minimum_required (VERSION 3.4)
set (PROJECT_NAME libgmsd)
set (LIB_NAME libgmsd)
set (LIB_TYPE STATIC)
set (TEST_NAME gmsd_tests)
set (BIN_NAME gmsd)

set (ADDL_LINK_NAMES "")
if (UNIX AND NOT WIN32)
	set (ADDL_LINK_NAMES "m")
endif()

include(cmake/get_cpm.cmake)
CPMAddPackage(
  NAME "PackageProject"
  GIT_REPOSITORY "https://github.com/TheLartians/PackageProject.cmake"
  GIT_TAG "v1.6.0")



set(LIBGMSD_STANDALONE_PROJECT OFF)
if (CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    set(LIBGMSD_STANDALONE_PROJECT ON)
endif()

option(LIBGMSD_TEST "Enable libgmsd test" ${LIBGMSD_STANDALONE_PROJECT})
option(LIBGMSD_EXE "Enable libgmsd executable" ${LIBGMSD_STANDALONE_PROJECT})

project (
    ${PROJECT_NAME}
    VERSION 0.1.0
)

# Common 3rdparty for exe and test app
file(GLOB 3RDPARTY_SOURCES "3rdparty/*.c")

# LIB
file(GLOB LIB_SOURCES "src/*.c")
add_library(${LIB_NAME} ${LIB_TYPE} ${LIB_SOURCES})
target_include_directories(${LIB_NAME} PUBLIC 
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include/${PROJECT_NAME}-${PROJECT_VERSION}>)
set_property(TARGET ${LIB_NAME} PROPERTY POSITION_INDEPENDENT_CODE ON)

packageProject(
    NAME ${PROJECT_NAME}
    VERSION ${PROJECT_VERSION}
    BINARY_DIR ${PROJECT_BINARY_DIR}
    INCLUDE_DIR ${PROJECT_SOURCE_DIR}/include
    INCLUDE_DESTINATION include/${PROJECT_NAME}-${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)


# EXE

if(LIBGMSD_EXE)
file(GLOB BIN_SOURCES "bin/*.c")

add_executable(${BIN_NAME} ${BIN_SOURCES} ${3RDPARTY_SOURCES})
target_link_libraries( ${BIN_NAME} ${LIB_NAME} ${ADDL_LINK_NAMES} )
target_include_directories(${BIN_NAME} PRIVATE 3rdparty)
endif()

if(LIBGMSD_TEST)
# CTEST APP
enable_testing()

file(GLOB TEST_SOURCES "test/*.c")
file(GLOB TEST_FILES "test/*.bmp")
  
add_executable(${TEST_NAME} ${TEST_SOURCES} ${3RDPARTY_SOURCES})
target_link_libraries( ${TEST_NAME} ${LIB_NAME} ${ADDL_LINK_NAMES} )
target_include_directories(${TEST_NAME} PRIVATE 3rdparty)

add_custom_command(
        TARGET ${TEST_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${TEST_FILES}
                ${CMAKE_CURRENT_BINARY_DIR}				
				)
add_test( NAME ${TEST_NAME} COMMAND ${TEST_NAME} )
endif()