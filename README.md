# cpack_test - minimal test of CPack on macOS with g++

```
cd build
cmake -DCMAKE_CXX_COMPILER=g++-11 -DCMAKE_C_COMPILER=gcc-11 ..
cpack --verbose
```
