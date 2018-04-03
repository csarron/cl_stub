cmake_minimum_required(VERSION 3.9)
project(OpenCLStub)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(cl_stub SHARED cl_stub.c)
target_include_directories(cl_stub PRIVATE include)
if (ANDROID)
    target_link_libraries(cl_stub log)
endif ()

install(TARGETS "cl_stub" LIBRARY DESTINATION lib PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE)
install(FILES "include/cl_stub.h" DESTINATION include)
install(FILES "include/CL/cl.hpp" DESTINATION include)

add_executable(TestStub TestStub.cc)

if (ANDROID)
    set_property(TARGET TestStub PROPERTY COMPILE_FLAGS "-fpie")
    set_property(TARGET TestStub PROPERTY LINK_FLAGS "-pie")
elseif (UNIX AND NOT APPLE)
    set_property(TARGET TestStub PROPERTY LINK_FLAGS "-Wl,--no-as-needed -ldl")
endif ()

target_include_directories(TestStub PRIVATE include)
target_link_libraries(TestStub cl_stub)

