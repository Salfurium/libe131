cmake_minimum_required(VERSION 3.20)

INCLUDE (CheckIncludeFiles)

set(SERVER_NAME "${PROJECT_NAME}_example_server")
set(CLIENT_NAME "${PROJECT_NAME}_example_client")

set(SERVER_HEADER_DEPS "stdio.h;stdlib.h;stdint.h;err.h")
set(CLIENT_HEADER_DEPS "unistd.h")

CHECK_INCLUDE_FILES("${SERVER_HEADER_DEPS}" HAVE_SERVER_HEADERS)
CHECK_INCLUDE_FILES("${CLIENT_HEADER_DEPS}" HAVE_CLIENT_HEADERS)

if (NOT HAVE_SERVER_HEADERS OR  NOT HAVE_CLIENT_HEADERS)
    message(FATAL_ERROR "not all headers are present to build the examples")
endif()

add_executable("${SERVER_NAME}" test_server.c)
add_executable("${CLIENT_NAME}" test_client.c)

add_dependencies("${SERVER_NAME}" ${PROJECT_NAME})
add_dependencies("${CLIENT_NAME}" ${PROJECT_NAME})
