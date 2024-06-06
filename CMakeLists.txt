cmake_minimum_required (VERSION 3.27)
cmake_policy(SET CMP0091 NEW)

project ("CmakePlayground" CXX)

######
## project setup
######

# note: don't forget the kits for this come from C:\Users\abrady\AppData\Local\CMakeTools\cmake-tools-kits.json

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_CXX_EXTENSIONS OFF)

# add the executable
add_executable(SimpleCppProject main.cpp)