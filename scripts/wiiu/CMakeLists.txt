cmake_minimum_required(VERSION 3.21)
project(bimg_wiiu LANGUAGES CXX)

get_filename_component(BIMG_DIR "../.." ABSOLUTE)
set(BX_DIR "${BIMG_DIR}/../bx-wiiu" CACHE PATH "bx source directory")
if(NOT TARGET bx)
    add_subdirectory(${BX_DIR}/scripts/wiiu ${CMAKE_CURRENT_BINARY_DIR}/bx)
endif()

add_library(bimg STATIC
    ${BIMG_DIR}/src/image.cpp
    ${BIMG_DIR}/src/image_decode.cpp
    ${BIMG_DIR}/src/image_decode_wic.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_block_sizes.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_color_unquantize.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_decompress_symbolic.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_entry.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_image.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_integer_sequence.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_mathlib.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_mathlib_softfloat.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_partition_tables.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_percentile_tables.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_quantization.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_symbolic_physical.cpp
    ${BIMG_DIR}/3rdparty/astc-encoder/source/astcenc_weight_quant_xfer_tables.cpp
)
target_include_directories(bimg PUBLIC ${BIMG_DIR}/include PRIVATE
    ${BIMG_DIR}/3rdparty ${BIMG_DIR}/3rdparty/tinyexr/deps
    ${BIMG_DIR}/3rdparty/astc-encoder/include)
# Runtime images include NanoVG's PNG/JPEG files and sky2's EXR heightmap.
target_compile_definitions(bimg PRIVATE
    ASTCENC_DECOMPRESS_ONLY
    BIMG_CONFIG_DECODE_ASTC=1
    BIMG_CONFIG_PARSE_ENABLE=0
    BIMG_CONFIG_PARSE_PNG=1
    BIMG_CONFIG_PARSE_JPEG=1
    BIMG_CONFIG_PARSE_EXR=1
)
target_compile_options(bimg PRIVATE -fno-exceptions -fno-rtti)
target_link_libraries(bimg PUBLIC bx)
