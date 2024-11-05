cmake_minimum_required(VERSION 3.5)

project(lua54 VERSION 5.4 LANGUAGES C)

if (UNIX)
	add_definitions(-DLUA_USE_POSIX)
endif()

file (GLOB SOURCES "src/*.c")
list(REMOVE_ITEM SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/src/onelua.c")

add_library(lua54 STATIC ${SOURCES} ${HEADERS})
target_include_directories(lua54 PUBLIC include)

set_target_properties(lua54 PROPERTIES
		C_STANDARD 99
		C_STANDARD_REQUIRED YES
		C_EXTENSIONS NO
)

install(TARGETS lua54
		ARCHIVE DESTINATION lib
		LIBRARY DESTINATION lib
		RUNTIME DESTINATION bin
)
