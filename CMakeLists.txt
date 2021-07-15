cmake_minimum_required(VERSION 3.20)

INCLUDE (CheckIncludeFiles)

project(e131)

set(HEADERS_UNIVERSAL "stdio.h;stdint.h;stdbool.h;sys/types.h;string.h;errno.h;inttypes.h")
if(UNIX)
    set(HEADERS_PLATFORM "unistd.h;netdb.h;sys/socket.h;arpa/inet.h;netinet/in.h")
elseif(WIN32)
    set(HEADERS_PLATFORM "WinSock2.h;ws2ipdef.h")
    if(MSVC)
        set(HEADERS_PLATFORM "${HEADERS_PLATFORM};BaseTsd.h")
    endif()
endif()

CHECK_INCLUDE_FILES("${HEADERS_UNIVERSAL}" HAVE_UNIVERSAL_HEADERS)
CHECK_INCLUDE_FILES("${HEADERS_PLATFORM}" HAVE_PLATFORM_HEADERS)

if (NOT HAVE_UNIVERSAL_HEADERS)
    message(FATAL_ERROR "not all universal headers are present")
endif()

if (NOT HAVE_PLATFORM_HEADERS)
    message(FATAL_ERROR "not all platform headers are present")
endif()

set(LIB_SOURCE_DIR "lib/")
set(LIB_PUBLIC_INCLUDE_DIR "include/")

file(GLOB_RECURSE LIB_SOURCE_FILES "${LIB_SOURCE_DIR}*.c")
file(GLOB_RECURSE LIB_PRIVATE_INCLUDE_FILES "${LIB_SOURCE_DIR}*.h")
file(GLOB_RECURSE LIB_PUBLIC_INCLUDE_FILES "${LIB_PUBLIC_INCLUDE_DIR}*.h")

add_library(${PROJECT_NAME} ${LIB_SOURCE_FILES} ${LIB_PRIVATE_INCLUDE_FILES} ${LIB_PUBLIC_INCLUDE_FILES})

target_include_directories(${PROJECT_NAME} PUBLIC ${LIB_PUBLIC_INCLUDE_DIR})
target_include_directories(${PROJECT_NAME} PRIVATE ${LIB_SOURCE_DIR})

set_target_properties(${PROJECT_NAME}
    PROPERTIES
    PUBLIC_HEADER "${LIB_PUBLIC_INCLUDE_FILES}"
    PRIVATE_HEADER "${LIB_PRIVATE_INCLUDE_FILES}"
    LINKER_LANGUAGE C
)

install(TARGETS ${PROJECT_NAME})

if(BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()
