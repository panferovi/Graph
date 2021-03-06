cmake_minimum_required(VERSION 3.13.4)
project(Graph)

find_package(BISON REQUIRED)
find_package(FLEX REQUIRED)

flex_target(scanner
  ${CMAKE_CURRENT_SOURCE_DIR}/Driver/lexer.l
  ${CMAKE_CURRENT_BINARY_DIR}/lexer.cc
)

bison_target(parser
  ${CMAKE_CURRENT_SOURCE_DIR}/Driver/parser.y
  ${CMAKE_CURRENT_BINARY_DIR}/parser.cc
  COMPILE_FLAGS "--defines=${CMAKE_CURRENT_BINARY_DIR}/parser.tab.hh"
)

add_flex_bison_dependency(scanner parser)

add_executable(${PROJECT_NAME} 
  ${CMAKE_CURRENT_SOURCE_DIR}/main.cpp
  ${BISON_parser_OUTPUTS}
  ${FLEX_scanner_OUTPUTS}
)

target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_20)

target_include_directories(${PROJECT_NAME} 
  PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/Graph
  PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/Driver
  PRIVATE ${CMAKE_CURRENT_BINARY_DIR}
)
