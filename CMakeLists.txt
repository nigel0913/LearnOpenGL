cmake_minimum_required(VERSION 3.6)

project(LearnOpenGL)

set(CMAKE_CXX_STANDARD 11)

include_directories(
    /Users/xienanjie/BuildDev/include
    include
)

link_directories(
    /Users/xienanjie/BuildDev/lib
)

find_library(CocoaLib Cocoa)
find_library(OpenGLLib OpenGL)
find_library(IOKitLib IOKit)
find_library(CoreVideoLib CoreVideo)

mark_as_advanced(CocoaLib OpenGLLib IOKitLib CoreVideoLib)

set(SYS_LIBS ${CocoaLib} ${OpenGLLib} ${IOKitLib} ${CoreVideoLib})
set(APP_LIBS SOIL glfw3 GLEW)
set(LIBS ${SYS_LIBS} ${APP_LIBS})

set(CHAPTERS
    1.getting_started
#    2.lighting
    )

set(1.getting_started
    5.transformations)

set(2.lighting
    1.colors)

foreach(CHAPTER ${CHAPTERS})
  message(STATUS "CHAPTER = ${CHAPTER}")
  foreach(DEMO ${${CHAPTER}})
    message(STATUS "DEMO = ${DEMO}")
    file(GLOB SOURCE
        "src/${CHAPTER}/${DEMO}/*.h"
        "src/${CHAPTER}/${DEMO}/*.cpp"
        )
    set(NAME "${CHAPTER}_${DEMO}")
      message(STATUS "NAME = ${NAME}")
      message(STATUS "SOURCE = ${SOURCE}")
    add_executable(${NAME} ${SOURCE})
    target_link_libraries(${NAME} ${LIBS})
    set_target_properties(${NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/bin/${CHAPTER}")

    # copy shader files to build directory
    file(GLOB SHADERS
        "src/${CHAPTER}/${DEMO}/*.vs"
        "src/${CHAPTER}/${DEMO}/*.frag"
        "src/${CHAPTER}/${DEMO}/*.gs"
        )
    foreach(SHADER ${SHADERS})
      if(WIN32)
        # configure_file(${SHADER} "test")
        add_custom_command(TARGET ${NAME} PRE_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${SHADER} $<TARGET_FILE_DIR:${NAME}>)
      elseif(UNIX)
        file(COPY ${SHADER} DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/bin/${CHAPTER})
      endif(WIN32)

    endforeach(SHADER)
  endforeach(DEMO)
endforeach(CHAPTER)


#set(SOURCE_FILES main.cpp Shader.h)
#add_executable(LearnOpenGL ${SOURCE_FILES})
#
#target_link_libraries(LearnOpenGL ${LIBS})

