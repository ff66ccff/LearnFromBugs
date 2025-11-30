cmake_minimum_required(VERSION 4.0)
project(C C)

set(CMAKE_C_STANDARD 17)

add_executable(C main.c library.c)

target_link_libraries(C kernel32 user32)
