cmake_minimum_required(VERSION 2.6)

project(lz4)
set (lz4_VERSION_MAJOR 1)
set (lz4_VERSION_MINOR 0)
set (lz4_VERSION_PATCH 0)
set (lz4_VERSION_TAG "")
set (lz4_VERSION "${lz4_VERSION_MAJOR}.${lz4_VERSION_MINOR}.${lz4_VERSION_PATCH}${lz4_VERSION_TAG}")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wundef -Wno-implicit-function-declaration -std=c99")

option(BUILD_SHARED_LIBS "Build shared lib" ON)
option(BUILD_STATIC_LIBS "Build static lib" ON)
option(BUILD_LZ4DEMO32   "Build lz4demo32" OFF)

set(lz4_SOURCES
	lz4.c)

set(lz4demo_SOURCES
	bench.c
	lz4demo.c
	lz4.c)

if(BUILD_SHARED_LIBS)
	add_library(lz4 SHARED ${lz4_SOURCES})

	set_target_properties(lz4 PROPERTIES
		SOVERSION ${lz4_VERSION})

	install(TARGETS lz4 DESTINATION lib)
endif()

if(BUILD_STATIC_LIBS)
	add_library(lz4static STATIC ${lz4_SOURCES})

	set_target_properties(lz4static PROPERTIES
		OUTPUT_NAME lz4)

	install(TARGETS lz4static
		LIBRARY DESTINATION lib
		ARCHIVE DESTINATION lib)
endif()

add_executable(lz4demo ${lz4demo_SOURCES})

if(BUILD_LZ4DEMO32)
	add_executable(lz4demo32 ${lz4demo_SOURCES})

	set_target_properties(lz4demo32 PROPERTIES
		COMPILE_FLAGS "-m32"
		LINK_FLAGS "-m32")
endif()
