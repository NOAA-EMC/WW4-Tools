#+ -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- +
#| WAVEWATCH IV, open source, code management by NOAA / NWS |
#+ -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- +
#
#@file CMakeLists.txt
#@brief CMake build configuration for the WAVEWATCH IV tools (WW4-Tools).
#@details Defines the C++ build and the GoogleTest based unit tests. C++ sources
#live in src/ and C++ tests in tests/cpp/. Python code lives in python/ and is
#tested with pytest (see pyproject.toml).
#
#@copyright © 2026 National Weather Service, National Oceanic and Atmospheric
#Administration.
#NWS often uses Generative AI (GenAI) for code development and refactoring. Whenever GenAI is used, NWS requires a full human review of code before it is added to its repositories.
#@author Main Author(s) : Aldgisl (AI Persona), Hendrik L. Tolman
#@date Initial : 2026-11-20
#@date Last update : 2026-11-20

cmake_minimum_required(VERSION 3.20)

project(WW4_Tools VERSION 0.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Options for strict warnings, sanitizers, and testing
option(WW4_STRICT_WARNINGS "Enable strict compiler warnings and treat as errors" OFF)
option(WW4_USE_SANITIZERS "Enable address and undefined behavior sanitizers" OFF)
option(WW4_ENABLE_TESTING "Enable WW4-Tools test suite" ON)

# Common compile options for WW4-Tools targets
set(WW4_COMMON_COMPILE_OPTIONS "")
set(WW4_COMMON_LINK_OPTIONS "")

if(MSVC)
    if(WW4_STRICT_WARNINGS)
        list(APPEND WW4_COMMON_COMPILE_OPTIONS /W4 /WX)
    endif()
else()
    list(APPEND WW4_COMMON_COMPILE_OPTIONS -Wall -Wextra -Wpedantic)
    if(WW4_STRICT_WARNINGS)
        list(APPEND WW4_COMMON_COMPILE_OPTIONS -Werror)
    endif()
    if(WW4_USE_SANITIZERS)
        list(APPEND WW4_COMMON_COMPILE_OPTIONS -fsanitize=address,undefined)
        list(APPEND WW4_COMMON_LINK_OPTIONS -fsanitize=address,undefined)
    endif()
endif()

# Libraries and executables are added here as C++ tools are developed, e.g.:
#   add_library(ww4_tools_foo src/foo.cpp)
#   target_include_directories(ww4_tools_foo PUBLIC src)
#   target_compile_options(ww4_tools_foo PRIVATE ${WW4_COMMON_COMPILE_OPTIONS})
#   target_link_options(ww4_tools_foo PRIVATE ${WW4_COMMON_LINK_OPTIONS})

# Tests
if(WW4_ENABLE_TESTING)
    if(EXISTS "${CMAKE_SOURCE_DIR}/externals/googletest/CMakeLists.txt")
        # For Windows: Prevent overriding the parent project's compiler/linker settings
        set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
        add_subdirectory(externals/googletest EXCLUDE_FROM_ALL SYSTEM)
    else()
        message(FATAL_ERROR "googletest submodule not found in externals/googletest. Please run: git submodule update --init --recursive")
    endif()

    enable_testing()
    include(GoogleTest)

    # Helper function to define tests
    function(ww4_add_test test_name source_file dependency_lib)
        add_executable(${test_name} ${source_file})
        target_link_libraries(${test_name} PRIVATE ${dependency_lib} GTest::gtest_main)
        target_compile_options(${test_name} PRIVATE ${WW4_COMMON_COMPILE_OPTIONS})
        target_link_options(${test_name} PRIVATE ${WW4_COMMON_LINK_OPTIONS})
        gtest_discover_tests(${test_name})
    endfunction()

    # Register tests here, e.g.:
    #   ww4_add_test(test_foo tests/cpp/L1_test_foo.cpp ww4_tools_foo)
    #   ww4_add_test(test_foo tests/cpp/L1_test_foo.cpp ww4_tools_foo)
endif()
