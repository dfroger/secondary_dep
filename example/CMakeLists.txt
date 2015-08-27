project(SECONDARY_DEP)
cmake_minimum_required(VERSION 2.8)

find_library(FOO_LIBRARY NAMES foo)

add_executable(main main.c)
target_link_libraries(main ${FOO_LIBRARY})
