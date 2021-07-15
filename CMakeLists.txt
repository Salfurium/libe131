cmake_minimum_required(VERSION 3.20)

project(libE131)

add_library(e131 src/e131.c)
target_include_directories(e131 PUBLIC src/)
