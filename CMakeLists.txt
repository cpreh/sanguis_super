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

macro(
	disable_option
	OPTION_NAME
)
	set(
		${OPTION_NAME}
		OFF
		CACHE
		BOOL
		"Disable ${OPTION_NAME} by default"
	)
endmacro()

disable_option(
	ENABLE_EXAMPLES
)

disable_option(
	ENABLE_DOC
)

disable_option(
	ENABLE_TEST
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

disable_option(
	ENABLE_CAMERA
)

disable_option(
	ENABLE_CEGUI
)

disable_option(
	ENABLE_CG
)

disable_option(
	ENABLE_FONTBITMAP
)

disable_option(
	ENABLE_GRAPH
)

disable_option(
	ENABLE_LINE_DRAWER
)

disable_option(
	ENABLE_MODELMD3
)

disable_option(
	ENABLE_MODELOBJ
)

disable_option(
	ENABLE_OPENCL
)

disable_option(
	ENABLE_POSTPROCESSING
)

disable_option(
	ENABLE_PROJECTILE
)

disable_option(
	ENABLE_RESOURCE_TREE
)

disable_option(
	ENABLE_SCENIC
)

disable_option(
	ENABLE_SHADER
)

disable_option(
	ENABLE_WAVE
)

add_subdirectory(
	spacegameengine
)

# Make sure mizuiro and majutsu (which are header-only) are processed before sge
macro(
	add_sge_dep
	LIB_NAME
)
	add_dependencies(
		${LIBNAME}
		mizuiro_dummy
		majutsu_dummy
	)
endmacro()

if(
	ENABLE_SHARED
)
	add_sge_dep(
		sgecore
	)
endif()

if(
	ENABLE_STATIC
)
	add_sge_dep(
		sgecore_static
	)
endif()

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

make_config_path(
	alda
)

add_subdirectory(
	sanguis
)
