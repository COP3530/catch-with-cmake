cmake_minimum_required(VERSION 3.24)
project(Project1)

set(CMAKE_CXX_STANDARD 14)


#recommended by aman to match test environment
set(GCC_COVERAGE_COMPILE_FLAGS "-Wall -Werror")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${GCC_COVERAGE_COMPILE_FLAGS}" )

#pull in desired version of catch through cmake automatically, make it available
Include(FetchContent)
FetchContent_Declare(
        Catch2
        GIT_REPOSITORY https://github.com/catchorg/Catch2.git
        GIT_TAG        v3.4.0 # seems to work with CLion - if you run into problems, try an older version of catch2 v3
)
FetchContent_MakeAvailable(Catch2)

include_directories(test-unit)

#the first word is the name of the executble
# everything else are the files to include - yours will probably look a bit different than mine
add_executable(Main
        src/main.cpp # your main
        # the files below are *examples* to show what a finished project's CMake would look like
        # remove these and replace with your own source files
        src/AVL.cpp src/AVL.h src/Parser.cpp src/Parser.h src/StudentNode.h)

#this one doesn't include main, instead having your test.cpp
add_executable(Tests #this will be named Project1 by default - either change it to Tests or see the comment at the bottom of the file
        test-unit/test.cpp # your test file
        # the files below are *examples* to show what a finished project's CMake would look like
        # remove these and replace with your own source files
        src/AVL.cpp src/AVL.h src/Parser.cpp src/Parser.h src/StudentNode.h)
        
target_link_libraries(Tests PRIVATE Catch2::Catch2WithMain) #link catch to test.cpp file
# the name here must match that of your testing executable (the one that has test.cpp)
