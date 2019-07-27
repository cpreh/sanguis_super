cmake_minimum_required(
	VERSION
	2.8.12
)

project(
	sanguis-super
)

set(
	FCPPT_DONT_EXPORT
	TRUE
)

set(
	MIZUIRO_DONT_EXPORT
	TRUE
)

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

option(
	ENABLE_CLIENT
	"Build the client"
	ON
)

macro(
	force_option
	OPTION_NAME
	VALUE
)
	set(
		${OPTION_NAME}
		${VALUE}
		CACHE
		BOOL
		"Disable ${OPTION_NAME} by default"
	)

endmacro()

macro(
	enable_option
	OPTION_NAME
)
	force_option(
		${OPTION_NAME}
		ON
	)
endmacro()

macro(
	disable_option
	OPTION_NAME
)
	force_option(
		${OPTION_NAME}
		OFF
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

enable_option(
	ENABLE_BOOST
)

enable_option(
	ENABLE_FILESYSTEM
)

enable_option(
	ENABLE_SYSTEM
)

add_subdirectory(
	fcppt
)

list(
	APPEND
	CMAKE_MODULE_PATH
	"${CMAKE_CURRENT_SOURCE_DIR}/fcppt/cmake/modules"
)

make_config_path(
	fcppt
)

if(
	ENABLE_CLIENT
)
	add_subdirectory(
		libawl
	)
endif()

add_subdirectory(
	mizuiro
)

if(
	ENABLE_CLIENT
)
	# The library should be renamed to awl at some point
	set(
		awl_DIR
		"${CMAKE_BINARY_DIR}/libawl/config"
	)

	list(
		APPEND
		CMAKE_MODULE_PATH
		"${CMAKE_CURRENT_SOURCE_DIR}/libawl/CMakeModules"
	)
endif()

make_config_path(
	mizuiro
)

disable_option(
	ENABLE_BVH
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
	ENABLE_TOOLS
)

disable_option(
	ENABLE_WAVE
)

add_subdirectory(
	spacegameengine
)

add_subdirectory(
	alda
)

# Rename as well?
set(
	sge_DIR
	"${CMAKE_BINARY_DIR}/spacegameengine/config"
)

list(
	APPEND
	CMAKE_MODULE_PATH
	${CMAKE_CURRENT_SOURCE_DIR}/spacegameengine/cmake/modules/public
)

make_config_path(
	alda
)

add_subdirectory(
	sanguis
)
