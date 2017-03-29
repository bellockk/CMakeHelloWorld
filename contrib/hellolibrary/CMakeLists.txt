CMAKE_MINIMUM_REQUIRED( VERSION 3.5 )
PROJECT ( HelloLibrary VERSION 0.1.2 LANGUAGES CXX )

# Create a List of all Source code.  Do not use GLOB...
SET( SOURCE src/hello.cpp
            src/goodby.cpp )

# Create a Target for the static Library
ADD_LIBRARY( hello_library_static STATIC ${SOURCE} )
SET_TARGET_PROPERTIES( hello_library_static PROPERTIES
    OUTPUT_NAME hello )

# Create a Target for the shared Library
ADD_LIBRARY( hello_library_shared SHARED ${SOURCE} )
SET_TARGET_PROPERTIES( hello_library_shared PROPERTIES
    OUTPUT_NAME hello
    VERSION 0.1.2
    SOVERSION 0
    COMPILE_DEFINITIONS "MY_DEFINE"
    COMPILE_FLAGS "-DPUT_DEFINES_ELSEWHERE"
    LINK_FLAGS "-Bstatic -Bdynamic"
    ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib
    LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
    INSTALL_RPATH "${CMAKE_INSTALL_RPATH}:\$ORIGIN/../lib:\$ORIGIN"
    BUILD_WITH_INSTALL_RPATH TRUE )


INCLUDE_DIRECTORIES( include )
