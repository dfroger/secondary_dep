project(SECONDARY_DEP)
cmake_minimum_required(VERSION 2.8)


add_library(foo SHARED foo.c)
add_library(bar SHARED bar.c)

target_link_libraries(foo bar)

install(TARGETS foo DESTINATION lib)
install(TARGETS bar DESTINATION lib)
