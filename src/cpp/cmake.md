# CMake
```cmake
cmake_minimum_required(VERSION "3.12.0")
project("my_app" LANGUAAGES CXX)

add_executable("my_exec" "main.cpp")
add_library("my_lib" "lib.cpp")
target_compile_features("my_exec" PRIVATE "cxx_std_17")
set_target_properties("my_exec" PROPERTIES
	CXX_STANDARD_REQUIRED YES
	CXX_EXTENSIONS NO)
target_link_libraries("my_exec" PRIVATE my_lib)
target_include_directories("test_runner" SYTEM PRIVATE "cute")

enable_testing()
add_executable("test_runner", "Test.cpp")
add_test("tests")
```
- features, include paths, dependencies should be `PUBLIC`
```bash
#building
mkdir build
cd build
cmake ..
cmake --build .

#testing
cmake ..
cmake --build .
ctest --output-on_failure
```
