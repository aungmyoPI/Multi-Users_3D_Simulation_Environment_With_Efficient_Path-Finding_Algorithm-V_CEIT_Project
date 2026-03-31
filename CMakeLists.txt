cmake_minimum_required(VERSION 3.10)
project(MyOpenGLGame)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

#Get Necessary External Libraries
include(FetchContent)

FetchContent_Declare(
	glfw
	GIT_REPOSITORY https://github.com/glfw/glfw.git
	GIT_TAG 3.4
)
FetchContent_MakeAvailable(glfw)

set(GLAD_DIR "${CMAKE_SOURCE_DIR}/external/glad")

add_library(glad_loader STATIC
	${GLAD_DIR}/src/glad.c
)

target_include_directories(glad_loader PUBLIC
	${GLAD_DIR}/include
)

file(GLOB_RECURSE SOURCES CONFIGURE_DEPENDS
	"src/*.cpp"
	"src/core/*.cpp"
	"src/entities/*.cpp"
	"src/renderer/*.cpp"
	"src/utils/*.cpp"
)

add_executable(MyGame ${SOURCES})

target_include_directories(MyGame PRIVATE
	include/
	src/
	${glfw_SOURCE_DIR}/include
	${GLAD_DIR}/include
)

target_link_libraries(MyGame PRIVATE
	glfw
	glad_loader
	opengl32
)
