cmake_minimum_required(VERSION 2.8)

project(RESTLESS CXX)

if(NOT ${CMAKE_VERSION} LESS 3.2)
        set(CMAKE_CXX_STANDARD 11)
        set(CMAKE_CXX_STANDARD_REQUIRED ON)
else()
        message(STATUS "Checking compiler flags for C++14 support.")
        include (CheckCXXCompilerFlag)
        check_cxx_compiler_flag("-std=c++11" COMPILER_SUPPORTS_CXX11)
        if(COMPILER_SUPPORTS_CXX11)
                message(STATUS "C++11 is supported")
                if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
                        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -stdlib=libc++")
                else()
                        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
                endif()
        else()
                message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11/14 support. Please use a different C++ compiler")
        endif()
endif()


# set up include path
set(RESTFUL_LIBRARIES restful CACHE INTERNAL "")
set(RESTFUL_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/include CACHE INTERNAL "")

macro(restful_option OPTION_NAME OPTION_TEXT OPTION_DEFAULT)
    option(${OPTION_NAME} ${OPTION_TEXT} ${OPTION_DEFAULT})
    if(DEFINED ENV{${OPTION_NAME}})
        # Allow setting the option through an environment variable
        set(${OPTION_NAME} $ENV{${OPTION_NAME}})
    endif()
    if(${OPTION_NAME})
        add_definitions(-D${OPTION_NAME})
    endif()
    message(STATUS "  ${OPTION_NAME}: ${${OPTION_NAME}}")
endmacro()

message(STATUS "Restful CMake Options")
message(STATUS "=======================================================")

restful_option(BUILD_RESTFUL_TESTS "Set to ON to build restless tests." OFF)
restful_option(GENERATE_COVERAGE "Set to ON to generate coverage reports." OFF)
restful_option(USE_SYSTEM_GTEST "Set to ON to generate coverage reports." OFF)
restful_option(INSECURE_CURL "Set to ON to disable verification of SSL certificates." OFF)
restful_option(USE_LOCAL_CURL "If ON, this project will look in the system paths for an installed curl library" OFF)

# set up tests

if(BUILD_RESTLESS_TESTS)
    enable_testing()
endif()
# 3rd party
add_subdirectory(${PROJECT_SOURCE_DIR}/ext)

# set up source directory
add_subdirectory(src)

if(BUILD_RESTFUL_TESTS)
    add_subdirectory(${PROJECT_SOURCE_DIR}/test)
endif()
