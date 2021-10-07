```
cmake -DCMAKE_CXX_COMPILER=g++-11 -DCMAKE_C_COMPILER=gcc-11 ..
cpack --verbose 
```

Generates a .zip, but why doesn't it contain dependencies (system libs)?