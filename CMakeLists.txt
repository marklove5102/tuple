cmake_minimum_required (VERSION 3.15)

project (taocpp-tuple VERSION 1.0.0 LANGUAGES CXX)

# define a header-only library
add_library (tuple INTERFACE)
add_library (taocpp::tuple ALIAS tuple)
target_include_directories (tuple INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:${TAOCPP_TUPLE_INSTALL_INCLUDE_DIR}>
)

# get public headers files
file (GLOB_RECURSE public_header_files CONFIGURE_DEPENDS
  ${CMAKE_CURRENT_SOURCE_DIR}/include/tao/tuple/*.hpp
)
# mark as public headers
set_target_properties (tuple PROPERTIES
  PUBLIC_HEADER "${public_header_files}"
)

# features used by taocpp/tuple
target_compile_features (tuple INTERFACE cxx_std_11)

if (BUILD_TESTING AND PROJECT_IS_TOP_LEVEL)
  enable_testing ()
  add_subdirectory (src/test/tuple)
endif ()

include(GNUInstallDirs)

# install and export target
install (TARGETS tuple EXPORT tuple-targets)

install (EXPORT tuple-targets
  FILE taocpp-tuple-config.cmake
  NAMESPACE taocpp::
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/taocpp-tuple
)

install (DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install (FILES LICENSE DESTINATION ${CMAKE_INSTALL_DOCDIR})