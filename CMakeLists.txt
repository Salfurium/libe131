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

add_library(${PROJECT_NAME} lib/e131.c)
target_include_directories(e131 PUBLIC include/)
