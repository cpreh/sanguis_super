cmake_minimum_required(
	VERSION 2.8
)

# Sorry, installing from this configuration is currently not supported
set(
	CMAKE_INSTALL_PREFIX
	""
	CACHE
	FILEPATH
	"Disabled install prefix"
	FORCE
)

project(
	sanguis-super
)

macro(
	append_modules_path
	PROJECT_NAME
)
	set(
		CMAKE_MODULE_PATH
		"${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}/CMakeModules;${CMAKE_MODULE_PATH}"
	)
endmacro()

macro(
	make_config_path
	PROJECT_NAME
)
	set(
		${PROJECT_NAME}_DIR
		"${CMAKE_BINARY_DIR}/${PROJECT_NAME}/config"
	)
endmacro()

option(
	ENABLE_TEST
	"Enable testing"
	OFF
)

if(
	ENABLE_TEST
)
	enable_testing()
endif()

add_subdirectory(
	fcppt
)

append_modules_path(
	fcppt
)

make_config_path(
	fcppt
)

add_subdirectory(
	libawl
)

set(
	ENABLE_FCPPT
	ON
	CACHE
	BOOL
	"Build mizuiro using fcppt"
)

add_subdirectory(
	mizuiro
)

add_subdirectory(
	majutsu
)

# The library should be renamed to awl at some point
set(
	awl_DIR
	"${CMAKE_BINARY_DIR}/libawl/config"
)

append_modules_path(
	libawl
)

make_config_path(
	mizuiro
)

make_config_path(
	majutsu
)

add_subdirectory(
	spacegameengine
)

# Make sure mizuiro and majutsu (which are header-only) are processed before sge
add_dependencies(
	sgecore
	mizuiro_dummy
	majutsu_dummy
)

add_subdirectory(
	alda
)

# Rename as well?
set(
	sge_DIR
	"${CMAKE_BINARY_DIR}/spacegameengine/config"
)

append_modules_path(
	spacegameengine
)

set(
	STATECHART_INCLUDE_DIR
	"${CMAKE_CURRENT_SOURCE_DIR}/statechart/include"
	CACHE
	FILEPATH
	"Statechart include directory"
)

make_config_path(
	alda
)

add_subdirectory(
	sanguis
)
