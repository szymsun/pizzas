cmake_minimum_required(VERSION 4.2.0)
project(Pizzas)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/03597a01ee50ed33e9dfd640b249b4be3799d395.zip
)
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)
include(GoogleTest)

set(SRC_LIST 
    ./src/Funghi.cpp ./src/Funghi.hpp 
    ./src/Margherita.cpp ./src/Margherita.hpp 
    ./src/Pizza.cpp ./src/Pizza.hpp 
    ./src/Pizzeria.cpp ./src/Pizzeria.hpp)

set(TEST_LIST ./test/PizzeriaTest.cpp ./test/mocks/PizzaMock.hpp)

add_executable(${PROJECT_NAME} ./src/main.cpp)
add_library(${PROJECT_NAME}-lib STATIC ${SRC_LIST})

add_executable(${PROJECT_NAME}-ut ./test/test_main.cpp)
target_link_libraries(${PROJECT_NAME}-ut PRIVATE ${PROJECT_NAME}-lib gtest_main gmock_main)

target_link_libraries(${PROJECT_NAME} ${PROJECT_NAME}-lib)
target_link_libraries(${PROJECT_NAME} pthread)

# For Windows: Prevent overriding the parent project's compiler/linker settings

# TODO: Write proper build system :)


