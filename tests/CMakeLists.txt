cmake_minimum_required(VERSION 3.10)
project(CuCryptoTests)


find_library(CUDART_LIBRARY cudart ${CMAKE_CUDA_IMPLICIT_LINK_DIRECTORIES})
# Find GoogleTest package
find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})
# Fetch GoogleTest
# include(FetchContent)
# FetchContent_Declare(
#   googletest
#   URL https://github.com/google/googletest/archive/refs/tags/release-1.11.0.zip
# )
# For Windows: Prevent overriding the parent project's compiler/linker settings
# set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
# FetchContent_MakeAvailable(googletest)

# Add the CuCrypto library path
include_directories(${CMAKE_SOURCE_DIR}/include)

# Add test files
set(TEST_SOURCES
    main.cu
    test_keccak.cu
    test_sha256.cu
    test_ripemd160.cu
    test_blake2.cu
)

# Create a single executable for all test files
add_executable(all_tests ${TEST_SOURCES})
target_link_libraries(all_tests PUBLIC ${GTEST_LIBRARIES} ${CUDART_LIBRARY} pthread CuCrypto)

# Add the test executable to CTest
add_test(NAME all_tests COMMAND all_tests)

# Create an executable for each test file
# foreach(TEST_SOURCE ${TEST_SOURCES})
#     get_filename_component(TEST_NAME ${TEST_SOURCE} NAME_WE)
#     add_executable(${TEST_NAME} ${TEST_SOURCE})
#     target_link_libraries(${TEST_NAME} PRIVATE ${GTEST_LIBRARIES} CuCrypto)
#     add_test(NAME ${TEST_NAME} COMMAND ${TEST_NAME})
# endforeach()