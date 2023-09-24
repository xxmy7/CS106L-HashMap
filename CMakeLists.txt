cmake_minimum_required(VERSION 3.22)
project(CS106L_HashMap)

set(CMAKE_CXX_STANDARD 17)

include_directories(.)

add_executable(hashmap.cpp
        hashmap.h
        hashmap_iterator.h
        main.cpp
        test_settings.cpp
        tests.cpp)
