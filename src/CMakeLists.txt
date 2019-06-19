file(GLOB SRC_FILES *.cpp)
include_directories(${RESTFUL_INCLUDE_DIRS}
                    ${CURL_INCLUDE_DIRS})
add_library(${RESTFUL_LIBRARIES} ${SRC_FILES})
target_link_libraries(${RESTFUL_LIBRARIES} ${CURL_LIBRARIES})

install (TARGETS restful DESTINATION bin)
install (FILES ${SRC_HEADER} DESTINATION include)
