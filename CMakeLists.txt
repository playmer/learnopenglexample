cmake_minimum_required(VERSION 3.19.0 FATAL_ERROR) 

# install vcpkg into our project
include(FetchContent)
FetchContent_Declare(
    vcpkg
    GIT_REPOSITORY "https://github.com/microsoft/vcpkg.git"
    GIT_TAG "48369f2a2aa00f7311ca8417cfd0fbda4bcc41e9" # 2022-08-01
)
if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
    FetchContent_Populate(vcpkg)
    set(CMAKE_TOOLCHAIN_FILE "${vcpkg_SOURCE_DIR}/scripts/buildsystems/vcpkg.cmake")
endif()

# Declare project and the languages you're compiling. (*.cpp files will be compiled as C++, *.c as C)
project(learnopenglexample VERSION 1.0.0 LANGUAGES C CXX)

# Go find the needed libraries for learnopengl.com
find_package(OpenGL REQUIRED)
find_package(glad CONFIG REQUIRED)
find_package(glfw3 CONFIG REQUIRED)

# Declare the executable to build.
add_executable(learnopenglexample)

# Add sources to the exe to build.
target_sources(learnopenglexample 
PRIVATE
    src/main.c
)

# Add libraries for the exe to link against
target_link_libraries(learnopenglexample 
PRIVATE 
    OpenGL::GL
    glad::glad
    glfw
)

# Some silly VS setting that is  better on than off.
set_property(GLOBAL PROPERTY USE_FOLDERS ON)  

# Sets the project to run when you hit F5 in VS
set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT PadInput)