cmake_minimum_required(VERSION 3.9 FATAL_ERROR)
project(cpcTest)

#disallow in-source build
if(CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_CURRENT_BINARY_DIR)
	message(FATAL_ERROR
	"cpcTest requires an out-of-source build. Please create a separate"
	"binary directory and run CMake there")
endif()

#set a default build type if none was specified
get_property(generator_is_multi_config GLOBAL PROPERTY GENERATOR_IS_MULTI_CONFIG)

if( NOT CMAKE_BUILD_TYPE AND NOT generator_is_multi_config)
	message(STATUS "Setting build type to 'Debug' as none was specified")
	set(CMAKE_BUILD_TYPE Debug CACHE STRING "choose the type of build" FORCE)
	#set the possible value for cmake-gui
	set_property(CACHE CMAKE_BUILD_TYPE
		PROPERTY
			STRINGS "Debug" "Release" "MinSizeRel" "RelWidthDebInfo")
endif()

#choose static or shared libs
option (IS_BUILD_SHARED_LIBS "build with shared libraries" ON)
set(BUILD_SHARD_LIBS "${IS_BUILD_SHARED_LIBS}")
set(CMAKE_CXX_VISIBILITY_PRESET "hidden")
set(CMAKE_VISIBILITY_INLINES_HIDDEN 1)

#set up our directory structure for output libraries and binaries
include(GNUInstallDirs)
if(NOT CMAKE_RUNTIME_OUTPUT_DIRECTORY)
	set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_INSTALL_BINDIR}")
endif()

if(NOT CMAKE_LIBRARY_OUTPUT_DIRECTORY)
	set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR}")
endif()

if(NOT CMAKE_ARCHIVE_OUTPUT_DIRECTORY)
	set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR}")
endif()

#add sub directories
add_subdirectory(ITK)
add_subdirectory(VTK)
