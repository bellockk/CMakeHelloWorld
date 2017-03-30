CMAKE_MINIMUM_REQUIRED( VERSION 3.5 )

SET( MAJOR 1 )
SET( MINOR 0 )
SET( PATCH 0 )
PROJECT ( HelloLibrary VERSION "${MAJOR}.${MINOR}.${PATCH}" LANGUAGES CXX )

# Create a List of all Source code.  Do not use GLOB...
SET( SOURCE src/hello.cpp
            src/goodby.cpp )

# Create a Target for the static Library
ADD_LIBRARY( hello_library_static STATIC ${SOURCE} )
TARGET_COMPILE_FEATURES( hello_library_static 
    PUBLIC cxx_auto_type
    PRIVATE cxx_variadic_templates )
SET_TARGET_PROPERTIES( hello_library_static PROPERTIES
    OUTPUT_NAME hello 
    POSITION_INDEPENDENT_CODE TRUE )
TARGET_INCLUDE_DIRECTORIES( hello_library_static PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
    PRIVATE src )

# Create a Target for the shared Library
ADD_LIBRARY( hello_library_shared SHARED ${SOURCE} )
TARGET_COMPILE_FEATURES( hello_library_shared
    PUBLIC cxx_auto_type
    PRIVATE cxx_variadic_templates )
SET_TARGET_PROPERTIES( hello_library_shared PROPERTIES
    OUTPUT_NAME hello
    VERSION "${MAJOR}.${MINOR}.${PATCH}"
    SOVERSION "${MAJOR}"
    COMPILE_DEFINITIONS "MAJOR=${MAJOR};MINOR=${MINOR};PATCH=${PATCH}"
    # COMPILE_FLAGS "-DPUT_DEFINES_ELSEWHERE"
    # LINK_FLAGS "-remember-platform-independent"
    ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib
    LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
    INSTALL_RPATH "${CMAKE_INSTALL_RPATH}:\$ORIGIN/../lib:\$ORIGIN"
    BUILD_WITH_INSTALL_RPATH TRUE )
TARGET_INCLUDE_DIRECTORIES( hello_library_shared PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
    PRIVATE src )

# Make Install Target
INSTALL( TARGETS hello_library_static hello_library_shared
    EXPORT HelloLibConfig
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin)
INSTALL( DIRECTORY include/ DESTINATION include )
INSTALL( EXPORT HelloLibConfig DESTINATION share/HelloLib/cmake )
EXPORT( TARGETS hello_library_static hello_library_shared FILE 
    HelloLibConfig.cmake )

