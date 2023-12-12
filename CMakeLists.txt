cmake_minimum_required(VERSION 3.10)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(kh)

add_library(kh_lib INTERFACE)
target_include_directories(kh_lib INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> $<INSTALL_INTERFACE:include>)

target_compile_features(kh_lib INTERFACE cxx_std_17)

include(FetchContent)
FetchContent_Declare(
  googletest
  GIT_REPOSITORY https://github.com/google/googletest.git
  GIT_TAG release-1.12.1
)
FetchContent_MakeAvailable(googletest)

enable_testing()

file(GLOB kh_test_src "test/*.cc")
add_executable(
  kh_test
  ${kh_test_src}
)
target_link_libraries(kh_test kh_lib)

target_link_libraries(
  kh_test
  GTest::gtest_main
)

include(GoogleTest)
gtest_discover_tests(kh_test)
