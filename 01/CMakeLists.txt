cmake_minimum_required(VERSION 3.2)

project(lab1 VERSION 0.0.1$ENV{TRAVIS_BUILD_NUMBER})

find_package(Boost COMPONENTS unit_test_framework REQUIRED)

message(${CMAKE_PROJECT_NAME}_test)

configure_file(version.h.in ${PROJECT_SOURCE_DIR}/version.h)

add_executable(${CMAKE_PROJECT_NAME} main.cpp)

add_executable(${CMAKE_PROJECT_NAME}_test test_version.cpp)

set_target_properties(${CMAKE_PROJECT_NAME} ${CMAKE_PROJECT_NAME}_test PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
    COMPILE_OPTIONS "-Wpedantic;-Wall;-Wextra"
)

set_target_properties(${CMAKE_PROJECT_NAME}_test PROPERTIES
    COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
    INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
)

target_link_libraries(${CMAKE_PROJECT_NAME}_test
    ${Boost_LIBRARIES}
)

set(CPACK_GENERATOR DEB)
set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")
set(CPACK_PACKAGE_CONTACT shebdim@gmail.com)
set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
include(CPack)

enable_testing()
add_test(helloworld_tests ${CMAKE_PROJECT_NAME}_test)
