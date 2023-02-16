cmake_minimum_required (VERSION 3.0)

project(hellocmake)

add_library(hellolib SHARED hello.cpp)
add_executable(a.out main.cpp hello.cpp)
target_link_libraries(a.out PUBLIC hellolib)
