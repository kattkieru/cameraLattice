cmake_minimum_required(VERSION 3.16)
project(tcCameraLattice CXX)

cmake_policy(SET CMP0048 NEW)
set(CMAKE_CXX_STANDARD 17 )

set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")
set(CMAKE_INSTALL_PREFIX "${PROJECT_SOURCE_DIR}")

find_package(Maya REQUIRED)

add_definitions(-DGL_SILENCE_DEPRECATION)

include_directories(${PROJECT_SOURCE_DIR}/include)

set(SOURCE_FILES
	source/PluginMain.cpp
	source/cameraLattice.cpp
	source/cameraLatticeInfluenceLocator.cpp
	source/cameraLatticeTranslator.cpp
)

add_library(tcCameraLattice SHARED ${SOURCE_FILES})
set_target_properties(tcCameraLattice PROPERTIES PREFIX "")

if (APPLE)
	set(OPENGL "-framework OpenGL")
else()
	## ??
endif()

target_link_libraries(${PROJECT_NAME} PRIVATE Maya::Maya Maya::tbb ${OPENGL})

target_include_directories(${PROJECT_NAME} 
	PRIVATE Maya::Maya
	PUBLIC "${CMAKE_CURRENT_BINARY_DIR}" "${CMAKE_CURRENT_SOURCE_DIR}"
)
MAYA_PLUGIN(${PROJECT_NAME})

install(
	TARGETS
		tcCameraLattice
		${MAYA_TARGET_TYPE}
	DESTINATION
		${CMAKE_INSTALL_PREFIX}/plug-ins
)
