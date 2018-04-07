cmake_minimum_required(VERSION 3.4)
project(CLStub)

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

add_executable(test_stub test_stub.cc cl_stub.c)

if (UNIX AND NOT APPLE) # Linux system
    set_property(TARGET test_stub PROPERTY LINK_FLAGS "-Wl,--no-as-needed -ldl")
endif ()

target_include_directories(test_stub PRIVATE include)
if (ANDROID)
    target_link_libraries(test_stub log)
endif ()