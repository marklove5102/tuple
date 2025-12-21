cmake_minimum_required (VERSION 3.15)
project (taocpp-tuple-tests LANGUAGES CXX)

set (test_sources
  tuple_benchmark.cpp
  tuple_cat.cpp
  tuple_main.cpp
)

# file (GLOB ...) is used to validate the above list of test_sources
file (GLOB glob_test_sources RELATIVE ${CMAKE_CURRENT_LIST_DIR} *.cpp)

foreach (testsourcefile ${test_sources})
  if (${testsourcefile} IN_LIST glob_test_sources)
    list (REMOVE_ITEM glob_test_sources ${testsourcefile})
  else ()
    message (SEND_ERROR "File ${testsourcefile} is missing from src/test/tuple")
  endif ()

  get_filename_component (exename ${testsourcefile} NAME_WE)
  set (exename "tao-tuple-test-${exename}")
  add_executable (${exename} ${testsourcefile})
  target_link_libraries (${exename} PRIVATE taocpp::tuple)
  target_compile_features(${exename} PRIVATE cxx_std_11)
  add_test (NAME ${exename} WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../../.. COMMAND ${CMAKE_CURRENT_BINARY_DIR}/${exename})
endforeach (testsourcefile)

if (glob_test_sources)
  foreach (ignored_source_file ${glob_test_sources})
    message (SEND_ERROR "File ${ignored_source_file} in src/test/tuple is ignored")
  endforeach (ignored_source_file)
endif ()
