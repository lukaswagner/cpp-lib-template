cmake_minimum_required(VERSION 3.6)

# project settings

project(lib CXX)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# sources

set(source_dir src)
set(include_dir include)
set(generated_header_dir ${CMAKE_BINARY_DIR}/generated)

set(export_header
    ${generated_header_dir}/lib_export.hpp)

set(public_headers
	${include_dir}/lib.hpp
    ${export_header})

set(sources
	${source_dir}/lib.cpp)

set(example_sources
    ${source_dir}/example.cpp)

# lib target

include_directories(${generated_header_dir})
add_library(lib SHARED ${public_headers} ${sources})

include(GenerateExportHeader)
generate_export_header(lib
    BASE_NAME lib
    EXPORT_MACRO_NAME LIB_EXPORT
    EXPORT_FILE_NAME ${export_header}
    STATIC_DEFINE LIB_BUILT_AS_STATIC)

# install settings

set_target_properties(lib PROPERTIES PUBLIC_HEADER "${public_headers}")

include(GNUInstallDirs)
install(TARGETS lib
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

# example executable

option(BUILD_EXAMPLE "Build example executable." OFF)

if (${BUILD_EXAMPLE})
	add_executable(example ${example_sources})
    target_link_libraries(example lib)
    set(VS_STARTUP_PROJECT example)
endif()

# reharper style settings

if(EXISTS ${CMAKE_SOURCE_DIR}/dependencies/cpp-style-settings)
    add_subdirectory(dependencies/cpp-style-settings EXCLUDE_FROM_ALL)
endif()
