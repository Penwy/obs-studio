cmake_minimum_required(VERSION 3.24...3.25)

if(NOT TARGET OBS::obfuscate)
  add_subdirectory("${CMAKE_SOURCE_DIR}/libobs" "${CMAKE_BINARY_DIR}/libobs")
endif()

add_library(_inject-helper INTERFACE)

target_sources(_inject-helper INTERFACE inject-helper.c)

target_link_libraries(_inject-helper INTERFACE OBS::inject-library OBS::obfuscate)

legacy_check()

add_executable(inject-helper)
target_link_libraries(inject-helper PRIVATE _inject-helper)

include(cmake/32bit.cmake)

set_target_properties_obs(inject-helper PROPERTIES FOLDER plugins/win-capture OUTPUT_NAME inject-helper64 MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

add_dependencies(win-capture inject-helper)
