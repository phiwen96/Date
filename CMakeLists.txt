cmake_minimum_required(VERSION 3.16.0)
project(Date)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


file(GLOB SOURCES src/*.cpp)
file(GLOB HEADERS include/*.hpp)



include_directories(include)

add_library(Date ${SOURCES})

target_include_directories(${PROJECT_NAME} 
    PUBLIC 
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

target_precompile_headers(${PROJECT_NAME} PUBLIC src/precompiledHeaders.pch)



install(TARGETS ${PROJECT_NAME} DESTINATION lib
        EXPORT ${PROJECT_NAME}Targets)
install(FILES ${HEADERS} DESTINATION include/${PROJECT_NAME})

install(EXPORT ${PROJECT_NAME}Targets
  FILE ${PROJECT_NAME}Targets.cmake
  DESTINATION lib/cmake/${PROJECT_NAME}
)


include(CMakePackageConfigHelpers)
# generate the config file that is includes the exports
configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
  INSTALL_DESTINATION "lib/cmake/example"
  NO_SET_AND_CHECK_MACRO
  NO_CHECK_REQUIRED_COMPONENTS_MACRO
  )
# generate the version file for the config file
write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
  VERSION "${Tutorial_VERSION_MAJOR}.${Tutorial_VERSION_MINOR}"
  COMPATIBILITY AnyNewerVersion
)

# install the configuration file
install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
  DESTINATION lib/cmake/${PROJECT_NAME}
  )

export(EXPORT ${PROJECT_NAME}Targets
  FILE "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Targets.cmake"
)

# file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/include/${PROJECT_NAME})

# add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
#     COMMAND ${CMAKE_COMMAND} -E copy_if_different
#         "${CMAKE_CURRENT_SOURCE_DIR}/include/Date.hpp"
#         "${CMAKE_BINARY_DIR}/include/${PROJECT_NAME}"
#     COMMENT "Copying MyLib public headers to ${CMAKE_BINARY_DIR}/include/my_lib"
#     VERBATIM)