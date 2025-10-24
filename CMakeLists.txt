cmake_minimum_required(VERSION 3.10)
project(MyApp C)
file(GLOB_RECURSE SRC_FILES "*.c")

add_executable(MyApp ${SRC_FILES})
enable_testing()
add_test(NAME RunMyApp COMMAND MyApp)
