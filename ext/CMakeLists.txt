macro(set_cache_variable VAR_NAME VAR_DESCRIPTION)
    set(${VAR_NAME} ${${VAR_NAME}} CACHE INTERNAL ${VAR_DESCRIPTION})
    message(STATUS "Set ${VAR_NAME} to ${${VAR_NAME}}.")
endmacro()

# Code coverage
if(BUILD_RESTFUL_TESTS AND GENERATE_COVERAGE)
    set(CMAKE_BUILD_TYPE COVERAGE CACHE INTERNAL "Coverage enabled build")
    message(STATUS "Enabling gcov support")
    if(NOT "${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
        set(COVERAGE_FLAG "--coverage")
    endif()
    set(CMAKE_CXX_FLAGS_COVERAGE
        "-g -O0 ${COVERAGE_FLAG} -fprofile-arcs -ftest-coverage"
        CACHE STRING "Flags used by the C++ compiler during coverage builds."
        FORCE)
    set(CMAKE_C_FLAGS_COVERAGE
        "-g -O0 ${COVERAGE_FLAG} -fprofile-arcs -ftest-coverage"
        CACHE STRING "Flags used by the C compiler during coverage builds."
        FORCE)
    set(CMAKE_EXE_LINKER_FLAGS_COVERAGE
        ""
        CACHE STRING "Flags used for linking binaries during coverage builds."
        FORCE)
    set(CMAKE_SHARED_LINKER_FLAGS_COVERAGE
        ""
        CACHE STRING "Flags used by the shared libraries linker during coverage builds."
        FORCE)
    mark_as_advanced(
        CMAKE_CXX_FLAGS_COVERAGE
        CMAKE_C_FLAGS_COVERAGE
        CMAKE_EXE_LINKER_FLAGS_COVERAGE
        CMAKE_SHARED_LINKER_FLAGS_COVERAGE)
endif()

# Curl configuration
if(NOT USE_LOCAL_CURL)
    find_package(CURL)
endif()
if(USE_LOCAL_CURL OR NOT CURL_FOUND)
    message(STATUS "Not using system Curl, using built-in curl project instead.")
    add_subdirectory(curl)
    set(CURL_FOUND TRUE)
    set(CURL_LIBRARIES libcurl)
    set(CURL_INCLUDE_DIRS
        ${CURL_SOURCE_DIR}/include
        ${CURL_BINARY_DIR}/include/curl)
endif()

set_cache_variable(CURL_FOUND "Set if libcurl is found or built")
set_cache_variable(CURL_LIBRARIES "Location of libcurl")
set_cache_variable(CURL_INCLUDE_DIRS "Location of curl include files")

# GTest configuration
if(BUILD_RESTFUL_TESTS)
    if(USE_SYSTEM_GTEST)
        find_package(GTest)
    endif()
    if(NOT USE_SYSTEM_GTEST OR NOT GTEST_FOUND)
        message(STATUS "Not using system gtest, using built-in googletest project instead.")
        add_subdirectory(gtest)
        set(GTEST_FOUND TRUE)
        set(GTEST_LIBRARIES gtest)
        set(GTEST_MAIN_LIBRARIES gtest_main)
        set(GTEST_BOTH_LIBRARIES gtest gtest_main)
        set(GTEST_INCLUDE_DIRS ${gtest_SOURCE_DIR}/include)
    endif()

    set_cache_variable(GTEST_FOUND "Set if libgtest was found or built")
    set_cache_variable(GTEST_LIBRARIES "Location of libgtest")
    set_cache_variable(GTEST_MAIN_LIBRARIES "Location of libgtest-main")
    set_cache_variable(GTEST_BOTH_LIBRARIES "Location of both gtest libraries")
    set_cache_variable(GTEST_INCLUDE_DIRS "Location of gtest include files")

    target_compile_options(gtest PUBLIC -DGTEST_USE_OWN_TR1_TUPLE)
endif()

# json
set(JSON_INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/ext/json/src)
