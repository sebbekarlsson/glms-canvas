cmake_minimum_required(VERSION 3.20)


# set (CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH};${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules")

include(FetchContent)

project(glms_canvas)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

file(
    GLOB
    sourcefiles
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/modules/*.c
)

#file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/assets DESTINATION ${CMAKE_CURRENT_BINARY_DIR})

#add_executable(glms_canvas_e ${sourcefiles})
list(FILTER sourcefiles EXCLUDE REGEX "main.c")
add_library(glms_canvas SHARED ${sourcefiles})
add_library(glms_canvas_static STATIC ${sourcefiles})


# set (GLMS_CANVAS_DEBUG_FLAGS -g -Wall)
set (GLMS_CANVAS_DEBUG_FLAGS )

# target_compile_options(glms_canvas_e PRIVATE ${GLMS_CANVAS_DEBUG_FLAGS})
target_compile_options(glms_canvas PRIVATE ${GLMS_CANVAS_DEBUG_FLAGS})
target_compile_options(glms_canvas_static PRIVATE ${GLMS_CANVAS_DEBUG_FLAGS})


target_include_directories(
  glms_canvas
    PUBLIC
      $<INSTALL_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
      $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/include>
      $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
      ${CMAKE_CURRENT_BINARY_DIR}/include
      ${CMAKE_CURRENT_SOURCE_DIR}/include)



target_include_directories(
  glms_canvas_static
    PUBLIC
      $<INSTALL_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
      $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/include>
      $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
      ${CMAKE_CURRENT_BINARY_DIR}/include
      ${CMAKE_CURRENT_SOURCE_DIR}/include)


find_library(GLFW_LIB glfw)


if (NOT GLFW_LIB)
    if (NOT TARGET glfw)
	FetchContent_Declare(
	glfw
	GIT_REPOSITORY https://github.com/glfw/glfw
	GIT_TAG "3.3.8"
	)
	FetchContent_MakeAvailable(glfw)
    endif()
endif()


FetchContent_Declare(
	glms_static
	URL /home/ianertson/workspace/glms
#	GIT_REPOSITORY https://github.com/sebbekarlsson/glms.git
	)
	FetchContent_MakeAvailable(glms_static)

set(GLMS_CANVAS_DEPS glms_static GL glfw GLEW)


target_link_libraries(glms_canvas ${GLMS_CANVAS_DEPS})
# target_link_libraries(glms_canvas_e ${GLMS_CANVAS_DEPS})
target_link_libraries(glms_canvas_static ${GLMS_CANVAS_DEPS})
