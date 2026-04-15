# Fetch googletest via FetchContent (replaces CIBC-Internal/googletest ExternalProject)
include(FetchContent)
FetchContent_Declare(
  googletest
  GIT_REPOSITORY https://github.com/google/googletest.git
  GIT_TAG v1.15.2
)
# Prevent googletest from overriding our compiler/linker options
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
set(INSTALL_GTEST OFF CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

# Definitions for Tests
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CLEAVER2_BINARY_DIR}/test/bin)

# test macro
function(NEWTEST name)
  add_executable(${name} ${name}.cpp ${ARGN})
  target_link_libraries(${name} GTest::gtest_main GTest::gmock cleaver)
  add_test(NAME ${name} COMMAND ${name})
endfunction()

add_subdirectory(cleaver)
add_subdirectory(cli)
