cmake_minimum_required(VERSION 3.28)
project(libfvad VERSION 1.0.1 LANGUAGES C)

option(ENABLE_EXAMPLES "build the fvadwav example program; requires libsndfile" OFF)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

add_subdirectory(src)
if(ENABLE_EXAMPLES)
    add_subdirectory(examples)
endif()

install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

# Export the package for use with FetchContent and find_package
install(EXPORT fvadTargets
    FILE fvadTargets.cmake
    NAMESPACE fvad::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/fvad
)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/fvadConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(
    ${CMAKE_CURRENT_LIST_DIR}/cmake/fvadConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/fvadConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/fvad
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/fvadConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/fvadConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/fvad
)

export(EXPORT fvadTargets
    FILE "${CMAKE_BINARY_DIR}/fvadTargets.cmake"
    NAMESPACE fvad::
)
