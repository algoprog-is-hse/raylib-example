cmake_minimum_required(VERSION 3.10.0)

add_executable(proj main.cpp lib.cpp)
# ^ Взять main.cpp и lib.cpp, собрать вместе, назвать proj_test (proj_test.exe)
target_link_libraries(proj PUBLIC default raylib-cpp)
# ^ Использовать в proj библиотеки raylib-cpp и default

# Так как VSCode любит при запуске неправильно указывать текущий каталог,
# мы создаём ссылку на каталог resources рядом с исполняемыми файлами
if(NOT EMSCRIPTEN)
add_custom_command(TARGET proj POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E $<IF:$<PLATFORM_ID:Windows>,copy_directory,create_symlink>
  "${CMAKE_CURRENT_SOURCE_DIR}/../resources" "$<TARGET_FILE_DIR:proj>/resources")
endif()