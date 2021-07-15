cmake_minimum_required(VERSION 3.20)

project(e131)

add_library(${PROJECT_NAME} lib/e131.c)
target_include_directories(e131 PUBLIC include/)
