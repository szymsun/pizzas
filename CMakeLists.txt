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



# for some reason the check.sh on remote has problems with seeing this part. when i ran it locally test passed just fine. WHAAAAAAAAAAAAT
set(SRC_LIST 
    ./src/Funghi.cpp
    ./src/Margherita.cpp  
    ./src/Pizza.cpp  
    ./src/Pizzeria.cpp )

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


