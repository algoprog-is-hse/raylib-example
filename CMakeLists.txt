cmake_minimum_required(VERSION 3.10.0)

project(Raylib_Example C CXX)
set(CMAKE_CXX_STANDARD 17) # CMAKE_CXX_STANDARD = 17
include(cmake/StandardProjectSettings.cmake)
# Link this 'library' to set the c++ standard / compile-time options requested
add_library(default INTERFACE)
target_compile_features(default INTERFACE cxx_std_17)

include(cmake/CompilerWarnings.cmake)
set_project_warnings(default)
if(EMSCRIPTEN)
  set(CMAKE_CXX_FLAGS "-s USE_GLFW=3 -s ASYNCIFY \
      --shell-file ${CMAKE_CURRENT_SOURCE_DIR}/raylib-cpp/vendor/raylib/src/shell.html\
      -s FORCE_FILESYSTEM=1 --preload-file ${CMAKE_CURRENT_SOURCE_DIR}/resources@resources")
  set(CMAKE_EXECUTABLE_SUFFIX .html)
endif()

add_subdirectory(raylib-cpp EXCLUDE_FROM_ALL)
# raylib выдаёт очень много предупреждений в своих загловочных файлах
# следующие две строки скрывают все предупреждения из его заголовочных файлов
get_target_property(PATH1 raylib-cpp INTERFACE_INCLUDE_DIRECTORIES)
set_property(TARGET raylib-cpp APPEND PROPERTY INTERFACE_SYSTEM_INCLUDE_DIRECTORIES "${PATH1}")
set_property(TARGET raylib-cpp APPEND PROPERTY INTERFACE_LINK_LIBRARIES raylib)

add_subdirectory(external/doctest)
include(external/doctest/doctest.cmake)
add_subdirectory(src)

# Так как VSCode любит при запуске неправильно указывать текущий каталог,
# мы создаём ссылку на каталог resources рядом с исполняемыми файлами
# file(CREATE_LINK "${CMAKE_CURRENT_SOURCE_DIR}/resources" "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/resources" COPY_ON_ERROR SYMBOLIC)