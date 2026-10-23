cmake_minimum_required(VERSION 3.21)
project(bimg_wiiu LANGUAGES CXX)

get_filename_component(BIMG_DIR "../.." ABSOLUTE)
set(BX_DIR "${BIMG_DIR}/../bx-wiiu" CACHE PATH "bx source directory")
if(NOT TARGET bx)
    add_subdirectory(${BX_DIR}/scripts/wiiu ${CMAKE_CURRENT_BINARY_DIR}/bx)
endif()

add_library(bimg STATIC ${BIMG_DIR}/src/image.cpp)
target_include_directories(bimg PUBLIC ${BIMG_DIR}/include)
target_compile_definitions(bimg PRIVATE BIMG_CONFIG_DECODE_ASTC=0)
target_compile_options(bimg PRIVATE -fno-exceptions -fno-rtti)
target_link_libraries(bimg PUBLIC bx)
