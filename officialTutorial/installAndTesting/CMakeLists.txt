cmake_minimum_required(VERSION 3.6.0)
project(project)

add_subdirectory(MathFunctions)
add_subdirectory(Tutorial)


# include(CTest)
# Turn on CMake testing capabilities
enable_testing()

add_test(TutorialComp25 ${PROJECT_BINARY_DIR}/Tutorial/bin/debug/Tutorial 25)
set_tests_properties (TutorialComp25 PROPERTIES PASS_REGULAR_EXPRESSION "25 is 5")

# # does it handle small numbers
add_test (TutorialSmall ${PROJECT_BINARY_DIR}/Tutorial/bin/debug/Tutorial 0.0001)
set_tests_properties (TutorialSmall PROPERTIES PASS_REGULAR_EXPRESSION "0.0001 is 0.01")

# # does the usage message work?
add_test (TutorialUsage ${PROJECT_BINARY_DIR}/Tutorial/bin/debug/Tutorial)
set_tests_properties (TutorialUsage PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number")

# does it handle negative numbers
# add_test(TutorialNegative ${PROJECT_BINARY_DIR}/Tutorial/bin/debug/Tutorial -25)
# set_tests_properties (TutorialNegative PROPERTIES PASS_REGULAR_EXPRESSION "-25 is 0")
