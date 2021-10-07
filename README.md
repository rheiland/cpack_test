# cpack_test - minimal test of CPack on macOS with g++

```
cd build
cmake -DCMAKE_CXX_COMPILER=g++-11 -DCMAKE_C_COMPILER=gcc-11 ..
cpack --verbose
```
On macOS, this generates `omp_hello-0.1.1-Darwin.zip` that is only about 2K in size (containing only the `omp_hello` binary file).

When I do a similar build on Windows:
```
cmake .. -G "Visual Studio 16 2019"
cmake --build . --config Release
cpack --verbose
```
it generates `omp_hello-0.1-1-win64.zip` that is about 525K in size because it also contains the bundled .dll system libs.
