macro(add_restful_test _TEST_NAME)
    add_executable(${_TEST_NAME}_test
        ${_TEST_NAME}.cpp)
    target_link_libraries(${_TEST_NAME}_test
            ${GTEST_LIBRARIES}
            ${RESTFUL_LIBRARIES})
    add_test(NAME restless_${_TEST_NAME}_test COMMAND ${_TEST_NAME}_test)
endmacro()

include_directories(get_unit_test
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${RESTFUL_INCLUDE_DIRS}
        ${GTEST_INCLUDE_DIRS}
        ${PROJECT_SOURCE_DIR}/ext/json/src
        )

add_restful_test(get)
add_restful_test(post)
add_restful_test(put)
add_restful_test(delete)
