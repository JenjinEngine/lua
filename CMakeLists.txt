cmake_minimum_required(VERSION 3.5)

project(lua51 VERSION 5.4 LANGUAGES C)

file (GLOB SOURCES "src/*.c")
# Remove oneluac.c from the list of sources
list(REMOVE_ITEM SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/src/onelua.c")

add_library(lua51 STATIC ${SOURCES} ${HEADERS})
target_include_directories(lua51 PUBLIC include)

set_target_properties(lua51 PROPERTIES
		C_STANDARD 99
		C_STANDARD_REQUIRED YES
		C_EXTENSIONS NO
)

install(TARGETS lua51
		ARCHIVE DESTINATION lib
		LIBRARY DESTINATION lib
		RUNTIME DESTINATION bin
)
