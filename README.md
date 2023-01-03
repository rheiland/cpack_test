# cpack_test - minimal test of CPack for distributable OpenMP pgm

```
cd build
rm -rf *
cmake -DCMAKE_CXX_COMPILER=clang++ -DCMAKE_C_COMPILER=clang ..
make
```

Trying to then use `cpack` to bundle into a .zip seems problematic:
```
cpack --verbose
```
On macOS, this generates `omp_hello-0.1.1-Darwin.zip` that is only about 2K in size (containing only the `omp_hello` binary file).

---

Note: the static (not dynamic) lib, libomp.a, is used, making the `omp_hello` larger in size.

```
M1P~/git/cpack_test$ ty CMakeLists.txt 
cmake_minimum_required (VERSION 3.21)
project (omp_hello)

set(CMAKE_CXX_STANDARD 11)
add_executable(omp_hello omp_hello.cpp)

#    "-DCMAKE_OSX_ARCHITECTURES=x86_64;arm64"
find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

# install(TARGETS "omp_hello" DESTINATION .)
# install(TARGETS omp_hello DESTINATION bin)
# install(DIRECTORY "${PROJECT_SOURCE_DIR}/config/" DESTINATION "config")

# /opt/homebrew/opt/libomp/lib/libomp.a
# link_directories(/opt/homebrew/opt/libomp/lib)

if (APPLE)
    include_directories(/opt/homebrew/opt/libomp/include)
    SET(EXTRA_LIBS /opt/homebrew/opt/libomp/lib/libomp.a)
endif (APPLE)

target_link_libraries(omp_hello ${EXTRA_LIBS})

set(CPACK_GENERATOR "ZIP")
include(CPack)
```

---

# When I do a similar build on Windows:
```
cmake .. -G "Visual Studio 16 2019"
cmake --build . --config Release
cpack --verbose
```
it generates `omp_hello-0.1-1-win64.zip` that is about 525K in size because it also contains the bundled .dll system libs.
