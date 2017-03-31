cmake_minimum_required(VERSION 3.6)
project(LearnOpenGL)

set(CMAKE_CXX_STANDARD 11)

include_directories(
    /Users/xienanjie/BuildDev/glfw-3.2.1/include
    /Users/xienanjie/BuildDev/glew-2.0.0/include
    /Users/xienanjie/BuildDev/SOIL/src
)

link_directories(
    /Users/xienanjie/BuildDev/glfw-3.2.1/build/src
    /Users/xienanjie/BuildDev/glew-2.0.0/build/lib
    /Users/xienanjie/BuildDev/SOIL/lib
)

set(SOURCE_FILES main.cpp Shader.h)
add_executable(LearnOpenGL ${SOURCE_FILES})

find_library(CocoaLib Cocoa)
find_library(OpenGLLib OpenGL)
find_library(IOKitLib IOKit)
find_library(CoreVideoLib CoreVideo)

mark_as_advanced(CocoaLib OpenGLLib IOKitLib CoreVideoLib)

set(SYS_LIBS ${CocoaLib} ${OpenGLLib} ${IOKitLib} ${CoreVideoLib})
target_link_libraries(LearnOpenGL ${SYS_LIBS})

target_link_libraries(LearnOpenGL SOIL)
target_link_libraries(LearnOpenGL glfw3)
target_link_libraries(LearnOpenGL GLEW)

