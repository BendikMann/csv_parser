cmake_minimum_required(VERSION 3.29)

project(csv_parser VERSION 1.0.0 DESCRIPTION "A simple C library for parsing CSV." LANGUAGES C)
add_library(csv_parser include/csv.h csv.c include/csv.h fread_csv_line.c split.c)
set_target_properties(csv_parser PROPERTIES VERSION ${PROJECT_VERSION})
set_target_properties(csv_parser PROPERTIES SOVERSION ${PROJECT_VERSION_MAJOR})
set_target_properties(csv_parser PROPERTIES PUBLIC_HEADER include/csv.h)

include_directories(include)

# Install library
install(TARGETS ${PROJECT_NAME} DESTINATION lib/${PROJECT_NAME})
install(FILES include/csv.h DESTINATION include/${PROJECT_NAME})

# Enable testing for the project
enable_testing()
add_executable(Test tests/test.c)
target_link_libraries(Test csv_parser)
