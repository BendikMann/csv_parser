cmake_minimum_required(VERSION 3.15)
project(csv_parser)

# make cache variables for install destinations
include(GNUInstallDirs)

# specify the C Standard
set(CMAKE_C_STANDARD 99)

# create library
add_library(csv_parser
        csv.h
        csv.c
        fread_csv_line.c
        split.c
)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# add include directories
target_include_directories(csv_parser
        PUBLIC
        "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>"
        "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>"
)

# install the target and create export-set
install(TARGETS csv_parser
        EXPORT csv_parserTargets
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

# install header file
install(FILES csv.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

# generate and install export file
install(EXPORT csv_parserTargets
        FILE csv_parserTargets.cmake
        NAMESPACE csv_parser::
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/csv_parser
)

# include CMakePackageConfigHelpers macro
include(CMakePackageConfigHelpers)

# set version
set(version 3.4.1)

set_property(TARGET csv_parser PROPERTY VERSION ${version})
set_property(TARGET csv_parser PROPERTY SOVERSION 3)
set_property(TARGET csv_parser PROPERTY
        INTERFACE_csv_parser_MAJOR_VERSION 3)
set_property(TARGET csv_parser APPEND PROPERTY
        COMPATIBLE_INTERFACE_STRING csv_parser_MAJOR_VERSION
)

# generate the version file for the config file
write_basic_package_version_file(
        "${CMAKE_CURRENT_BINARY_DIR}/csv_parserConfigVersion.cmake"
        VERSION "${version}"
        COMPATIBILITY AnyNewerVersion
)

# create config file
configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in
        "${CMAKE_CURRENT_BINARY_DIR}/csv_parserConfig.cmake"
        INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/csv_parser
)

# install config files
install(FILES
        "${CMAKE_CURRENT_BINARY_DIR}/csv_parserConfig.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/csv_parserConfigVersion.cmake"
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/csv_parser
)

# generate the export targets for the build tree
export(EXPORT csv_parserTargets
        FILE "${CMAKE_CURRENT_BINARY_DIR}/cmake/csv_parserTargets.cmake"
        NAMESPACE csv_parser::
)

# create tests for csv_parser

enable_testing()

include_directories(tests)

add_executable(Test tests/test.c
        )


target_link_libraries(Test csv_parser)
add_test(Test Test)

