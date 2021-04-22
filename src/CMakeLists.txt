cmake_minimum_required(VERSION 3.10.0)

add_executable(proj main.cpp lib.cpp)
# ^ Взять main.cpp и lib.cpp, собрать вместе, назвать proj_test (proj_test.exe)
target_link_libraries(proj PUBLIC default raylib-cpp)
# ^ Использовать в proj библиотеки raylib-cpp и default