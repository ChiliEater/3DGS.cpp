cmake_minimum_required(VERSION 3.26)
project(gs_viewer)

add_executable(gs_viewer main.cpp)

target_include_directories(gs_viewer PRIVATE third_party)

target_link_libraries(gs_viewer PRIVATE
    libenvpp::libenvpp
    gs_cpp)