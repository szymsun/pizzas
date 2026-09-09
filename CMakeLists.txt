cmake_minimum_required(VERSION 3.11.0)
project(Pizzas)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)



include(FetchContent)
FetchContent_Declare(
  googletest
  GIT_REPOSITORY https://github.com/google/googletest.git
  GIT_TAG main
)
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)




set(SRC_LIST 
    ./src/Funghi.cpp ./src/Funghi.hpp 
    ./src/Margherita.cpp ./src/Margherita.hpp 
    ./src/Pizza.cpp ./src/Pizza.hpp 
    ./src/Pizzeria.cpp ./src/Pizzeria.hpp)



add_executable(${PROJECT_NAME} ./src/main.cpp)
add_library(${PROJECT_NAME}-lib STATIC ${SRC_LIST})

target_link_libraries(${PROJECT_NAME} ${PROJECT_NAME}-lib)
target_link_libraries(${PROJECT_NAME} pthread)
target_include_directories(${PROJECT_NAME}-lib PUBLIC ./src)


enable_testing()

add_executable(${PROJECT_NAME}-ut ./test/PizzeriaTest.cpp)
target_link_libraries(${PROJECT_NAME}-ut PRIVATE ${PROJECT_NAME}-lib gtest_main gmock_main)

include(GoogleTest)
gtest_discover_tests(${PROJECT_NAME}-ut)


