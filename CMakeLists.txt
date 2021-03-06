cmake_minimum_required(VERSION 3.14)

project(cxpr VERSION "0.1" LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 17)

option(CXPR_BUILD_TESTS "Build and run cxpr tests" ON)

file(GLOB_RECURSE HEADERS "cxpr/*.h")

add_library(${PROJECT_NAME} INTERFACE)
target_sources(${PROJECT_NAME} INTERFACE ${HEADERS})
target_include_directories(${PROJECT_NAME} INTERFACE $<INSTALL_INTERFACE:include> $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/cxpr>)
target_compile_features(${PROJECT_NAME} INTERFACE cxx_std_17)

if(CXPR_BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()

install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

# This makes the project importable from the build directory
export(TARGETS ${PROJECT_NAME} FILE ${PROJECT_NAME}Config.cmake)
export(PACKAGE ${PROJECT_NAME})
