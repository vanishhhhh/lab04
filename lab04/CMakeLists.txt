cmake_minimum_required(VERSION 3.4)
project(lab03)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(formatter_lib)
add_subdirectory(formatter_ex_lib)
add_subdirectory(solver_lib)
add_subdirectory(hello_world)
add_subdirectory(solver)

install(TARGETS hello_world DESTINATION bin)
install(TARGETS formatter formatter_ex solver_lib DESTINATION lib)