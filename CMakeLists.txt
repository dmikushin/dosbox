cmake_minimum_required(VERSION 3.1)
  
project(dosbox)

set(PROJECT_VERSION_MAJOR 0)
set(PROJECT_VERSION_MINOR 1)
set(PROJECT_VERSION_PATCH 0)

find_package(SDL REQUIRED)
find_package(PNG REQUIRED)
find_package(OpenGL REQUIRED)

file(GLOB_RECURSE DOSBOX_SRCS
	${CMAKE_CURRENT_SOURCE_DIR}/src/cpu/*.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/src/debug/*.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/src/dos/*.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/src/fpu/*.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/src/gui/*.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/src/hardware/*.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/src/ints/*.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/src/misc/*.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/src/shell/*.cpp
	${CMAKE_CURRENT_SOURCE_DIR}/src/dosbox.cpp)

add_executable(${PROJECT_NAME} ${DOSBOX_SRCS})
target_compile_definitions(${PROJECT_NAME} PUBLIC HAVE_CONFIG_H GNU_SOURCE _REENTRANT)
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_include_directories(${PROJECT_NAME} PUBLIC ${SDL_INCLUDE_DIR})
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src/ints)
target_link_libraries(${PROJECT_NAME} PUBLIC ${SDL_LIBRARY} asound X11 z ${PNG_LIBRARY} ${OPENGL_LIBRARIES})
