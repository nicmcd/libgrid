cmake_minimum_required(VERSION 3.20)
project(libgrid)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

include(FindPkgConfig)

# libprim
pkg_check_modules(libprim REQUIRED IMPORTED_TARGET libprim)
  get_target_property(
  LIBPRIM_INC
  PkgConfig::libprim
  INTERFACE_INCLUDE_DIRECTORIES
)

add_library(
  grid
  SHARED
  ${PROJECT_SOURCE_DIR}/src/grid/Grid.cc
  ${PROJECT_SOURCE_DIR}/src/grid/Grid.h
  )

set_target_properties(
  grid
  PROPERTIES
  LINKER_LANGUAGE
  CXX
  POSITION_INDEPENDENT_CODE
  ON
  )

target_include_directories(
  grid
  PUBLIC
  ${PROJECT_SOURCE_DIR}/src
  ${LIBPRIM_INC}
  )

target_link_libraries(
  grid
  PkgConfig::libprim
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

configure_file(
  ${PROJECT_SOURCE_DIR}/libgrid.pc.in
  ${CMAKE_CURRENT_BINARY_DIR}/libgrid.pc
  @ONLY
  )
install(
  FILES
  ${CMAKE_CURRENT_BINARY_DIR}/libgrid.pc
  DESTINATION
  ${CMAKE_INSTALL_LIBDIR}/pkgconfig/
  )
