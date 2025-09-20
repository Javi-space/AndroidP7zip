cmake_minimum_required(VERSION 3.4.1)

# Add 16KB page size support using the older approach
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wl,-z,max-page-size=16384 -Wl,-z,common-page-size=16384")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wl,-z,max-page-size=16384 -Wl,-z,common-page-size=16384")

# Your existing flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DANDROID_NDK -fexceptions -DNDEBUG -D_REENTRANT -DENV_UNIX")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DBREAK_HANDLER -DUNICODE -D_UNICODE -DUNIX_USE_WIN_FILE -fPIC")

add_definitions("-w")
add_definitions("-fvisibility=hidden")
add_definitions("-Wno-error=c++11-narrowing")

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds it for you.
# Gradle automatically packages shared libraries with your APK.
file(GLOB_RECURSE NATIVE_SRCS *.c *.cpp)

include_directories(./ p7zip/CPP
        p7zip/CPP/Common
        p7zip/CPP/myWindows
        p7zip/CPP/include_windows)

add_library(p7zip SHARED ${NATIVE_SRCS})

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in the
# build script, prebuilt third-party libraries, or system libraries.
target_link_libraries(p7zip log)