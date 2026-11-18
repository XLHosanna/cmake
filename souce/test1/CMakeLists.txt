cmake_minimum_required(VERSION 3.10)
project(test1 VERSION 1.0.2)
add_executable(test1)
target_sources(test1 PRIVATE
    src/main.cpp
)

message(STATUS "PROJECT_NAME = ${PROJECT_NAME}")
message(STATUS "PROJECT_VERSION = ${PROJECT_VERSION}")
message(STATUS "PROJECT_SOURCE_DIR = ${PROJECT_SOURCE_DIR}")
message(STATUS "PROJECT_BINARY_DIR = ${PROJECT_BINARY_DIR}")
message(STATUS "test1_BINARY_DIR = ${test1_BINARY_DIR}")
message(STATUS "test1_SOURCE_DIR = ${test1_SOURCE_DIR}")
message("12test1_SOURCE_DIR = ${test1_SOURCE_DIR}")