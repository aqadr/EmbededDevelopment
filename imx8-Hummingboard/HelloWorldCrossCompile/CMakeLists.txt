# Set project's name
project(HelloWorld)

# Set version
set(SFMSIMULATION_VERSION_MAJOR 1)
set(SFMSIMULATION__VERSION_MINOR 0)
set(SFMSIMULATION__VERSION_PATCH 0)

cmake_minimum_required(VERSION 3.1)


message(STATUS "host system: ${CMAKE_HOST_SYSTEM_PROCESSOR}")
message(STATUS "system: ${CMAKE_SYSTEM_NAME}")
message(STATUS "system processor: ${CMAKE_SYSTEM_PROCESSOR}")

# ---------- Setup Library Output Directory -------------------------
message(STATUS "output dir: ${PROJECT_BINARY_DIR}")

# path to Findxxxx.cmake
list(APPEND CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH}  
     "${CMAKE_SOURCE_DIR}/toolchain/" 
)

# ---------- Setup Library Output Directory -------------------------
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY
    ${PROJECT_BINARY_DIR}/lib
    CACHE PATH
    "Single Build Directory for all Libraries"
)

# --------- Setup Static Library Output Directory -------------------
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY
    ${PROJECT_BINARY_DIR}/lib
    CACHE PATH
    "Single Build Directory for all static libraries."
)

# --------- Setup the Executable output Directory -------------------
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY
    ${PROJECT_BINARY_DIR}/bin
    CACHE PATH
    "Single Build Directory for all Executables."
)


# --------- Setup the Include output Directory ----------------------
set(CMAKE_INCLUDE_OUTPUT_DIRECTORY
    ${PROJECT_BINARY_DIR}/include
    CACHE PATH
    "Single Build Directory for all include files."
)

# ---------- Set additional compiler flags --------------------------
# set default C++ language version to 11
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
#GNU C++ compiler
if (UNIX AND CMAKE_COMPILER_IS_GNUCXX)
    if(NOT (${CMAKE_SYSTEM_PROCESSOR} MATCHES "arm*"))
        message(STATUS "setting additional compiler flags for non-arm processor")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -DNDEBUG -Wall -Wextra -fno-strict-aliasing")
        # Additional flags to boost performance
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DUNIX -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64 -mtune=generic")
   else() 
        message(STATUS "not setting additional compiler flag for arm processor")
   endif(NOT (${CMAKE_SYSTEM_PROCESSOR} MATCHES "arm*"))
    # to build on 64-bit OS
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")
endif(UNIX AND CMAKE_COMPILER_IS_GNUCXX)

# Clang
if (UNIX AND "${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -Weverything")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC -Weverything")
    if(NOT (${CMAKE_SYSTEM_PROCESSOR} MATCHES "arm*"))
        message(STATUS "setting additional compiler flags for non-arm processor")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-strict-aliasing -DUNIX -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64")
        # Additional flags to boost performance (only activate in release mode)
        set(CMAKE_CXX_FLAGS_RELEASE "-O2 -DNDEBUG -march=nocona -msse3 -mfpmath=sse -mno-avx -mtune=generic")
    else()
        message(STATUS "setting compiler flags for arm processor")
        set(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG")
    endif(NOT (${CMAKE_SYSTEM_PROCESSOR} MATCHES "arm*"))
endif(UNIX AND "${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

add_executable(hello source.cpp)
