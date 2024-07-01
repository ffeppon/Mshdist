cmake_minimum_required(VERSION 2.8)
set(CMAKE_BUILD_TYPE  "Release")
SET(CMAKE_C_FLAGS   "-w -fcommon -O3")
set(CMAKE_MACOSX_RPATH 1)
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

project(libMshdist)
file(                  GLOB_RECURSE sources sources/*)
file(                  GLOB_RECURSE headers sources/*.h)
add_library(           libMshdist SHARED ${sources})
set_target_properties( libMshdist PROPERTIES PREFIX "")

find_library(          Commons NAMES Commons HINTS "$ENV{HOME}/lib" required)
FIND_LIBRARY(          M_LIB m)
SET(                   EXTRA_LIBS ${M_LIB} "-lpthread")
include_directories(   "$ENV{HOME}/include")
target_link_libraries( libMshdist ${EXTRA_LIBS} ${Commons})

#Executable
add_executable(        mshdist sources/mshdist.c)
target_link_libraries( mshdist libMshdist ${Commons})

install(               TARGETS mshdist RUNTIME  DESTINATION "$ENV{HOME}/bin")
install(               TARGETS libMshdist LIBRARY DESTINATION "$ENV{HOME}/lib")
install(               FILES   ${headers} DESTINATION "$ENV{HOME}/include")
set_target_properties( mshdist PROPERTIES INSTALL_RPATH "$ENV{HOME}/lib")

add_custom_target(show_includes
    COMMAND ${CMAKE_CXX_COMPILER} -M ${CMAKE_SOURCE_DIR}/sources/mshdist.c
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    COMMENT "Showing includes for your_source_file.cpp"
)
