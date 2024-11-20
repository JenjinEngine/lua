cmake_minimum_required(VERSION 3.5)

project(lua54 VERSION 5.4 LANGUAGES C)

# Define platform-specific flags
if (UNIX)
    add_definitions(-DLUA_USE_POSIX)
endif()

# Collect source files, excluding 'onelua.c'
file(GLOB SOURCES "src/*.c")
list(REMOVE_ITEM SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/src/onelua.c")

# Define the library
add_library(lua54 STATIC ${SOURCES})

# Set include directories (applies to both lua54 and Lua::Lua)
target_include_directories(lua54 PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# Alias the target to Lua::Lua for compatibility
add_library(Lua::Lua ALIAS lua54)

# Set target properties
set_target_properties(lua54 PROPERTIES
    C_STANDARD 99
    C_STANDARD_REQUIRED YES
    C_EXTENSIONS NO
    POSITION_INDEPENDENT_CODE ON
)

# Install rules for the library and headers
install(TARGETS lua54
    EXPORT Lua54Targets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

install(DIRECTORY include/ DESTINATION include)

# Export the target for find_package
install(EXPORT Lua54Targets
    FILE Lua54Config.cmake
    NAMESPACE Lua::
    DESTINATION lib/cmake/Lua54
)

# Generate and install a version file
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/Lua54ConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)
install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/Lua54ConfigVersion.cmake"
    DESTINATION lib/cmake/Lua54
)

