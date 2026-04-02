cmake_minimum_required(VERSION 3.5)
project(formatter_lib)

add_library(formatter STATIC formatter.cpp)

target_include_directories(formatter PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})