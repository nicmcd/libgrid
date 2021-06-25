cmake_minimum_required(VERSION 3.10)
project(libgrid)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# libprim
find_library(LIBPRIM_A NAMES libprim.a PATHS ${CMAKE_INSTALL_RPATH})
if(LIBPRIM_A MATCHES "NOTFOUND")
  message(FATAL_ERROR "libprim.a not found")
endif()
string(REPLACE lib/libprim.a include/ LIBPRIM_INC ${LIBPRIM_A})
if(NOT EXISTS ${LIBPRIM_INC})
  message(FATAL_ERROR "libprim include dir doesn't exist")
endif()

add_library(
  grid
  STATIC
  ${PROJECT_SOURCE_DIR}/src/grid/Grid.cc
  ${PROJECT_SOURCE_DIR}/src/grid/Grid.h
  )

set_target_properties(
  grid
  PROPERTIES
  LINKER_LANGUAGE
  CXX
  )

target_include_directories(
  grid
  PUBLIC
  ${PROJECT_SOURCE_DIR}/src
  ${LIBPRIM_INC}
  )

target_link_libraries(
  grid
  ${LIBPRIM_A}
  )

include(GNUInstallDirs)

install(
  FILES
  ${PROJECT_SOURCE_DIR}/src/grid/Grid.h
  DESTINATION
  ${CMAKE_INSTALL_INCLUDEDIR}/grid/
  )

install(
  TARGETS
  grid
  )

