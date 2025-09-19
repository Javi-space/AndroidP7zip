cmake_minimum_required(VERSION 3.4.1)

# Your existing flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DANDROID_NDK -fexceptions -DNDEBUG -D_REENTRANT -DENV_UNIX")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DBREAK_HANDLER -DUNICODE -D_UNICODE -DUNIX_USE_WIN_FILE -fPIC")

add_definitions("-w")
add_definitions("-fvisibility=hidden")
add_definitions("-Wno-error=c++11-narrowing")

file(GLOB_RECURSE NATIVE_SRCS *.c *.cpp)
include_directories(./ p7zip/CPP
        p7zip/CPP/Common
        p7zip/CPP/myWindows
        p7zip/CPP/include_windows)

add_library(p7zip SHARED ${NATIVE_SRCS})

# Add 16KB page size support using target_link_options
target_link_options(p7zip PRIVATE 
    "-Wl,-z,max-page-size=16384" 
    "-Wl,-z,common-page-size=16384"
)

target_link_libraries(p7zip log)