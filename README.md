### Как собрать
После переключения на ветку `emscripten` выполните команду
```sh
git submodule update --init --recursive
```

Для сборки под браузер установите emscripten по интсрукции: (https://emscripten.org/docs/getting_started/downloads.html)

После этого в VSCode сделайте F1 - CMake: Edit User-Local CMake Kits. В открывшийся файл нужно дописать
```json
{
    "name": "Emscripten",
    "toolchainFile": "/path/to/emscripten/toolchain.cmake"
}
```
указав правильный путь до emscripten. Обычно он заканчивается на `upstream/emscripten/cmake/Modules/Platform/Emscripten.cmake`

После этого можно выбрать Emscripten в качестве компилятора и собрать проект. Для запуска в консоли перейти в папку, в которую cmake положил `proj.html`, и выполнить `python3 -m http.server 8000`. После этого можно в браузере открывать http://0.0.0.0:8000/
