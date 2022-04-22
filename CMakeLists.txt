cmake_minimum_required(VERSION 3.0) # guess
project(SourceSDK_TF2 CXX)

#
# Meta-project for compiling Source SDK static libraries
#
add_library(source_sdk_meta INTERFACE)
target_include_directories(source_sdk_meta INTERFACE
    "${CMAKE_CURRENT_LIST_DIR}/public"
    "${CMAKE_CURRENT_LIST_DIR}/public/mathlib"
    "${CMAKE_CURRENT_LIST_DIR}/public/tier0"
    "${CMAKE_CURRENT_LIST_DIR}/public/tier1"
)
if(UNIX AND NOT APPLE) # Linux
    target_compile_definitions(source_sdk_meta INTERFACE
        POSIX
        LINUX
        _LINUX
        GNUC  
    )
    target_compile_options(source_sdk_meta INTERFACE -msse)
elseif(WIN32)
    target_compile_definitions(source_sdk_meta INTERFACE
        _CRT_SECURE_NO_WARNINGS
    )
endif()
target_compile_definitions(source_sdk_meta INTERFACE
    RAD_TELEMETRY_DISABLED
)

#
# Mathlib
#
add_library(mathlib STATIC
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/3dnow.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/almostequal.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/anorms.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/bumpvects.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/color_conversion.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/halton.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/IceKey.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/imagequant.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/lightdesc.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/mathlib_base.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/polyhedron.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/powsse.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/quantize.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/randsse.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/simdvectormatrix.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/sparse_convolution_noise.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/spherical.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/sseconst.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/sse.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/ssenoise.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/vector.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/mathlib/vmatrix.cpp"
)
target_link_libraries(mathlib PUBLIC source_sdk_meta)

#
# Tier 0
# No sources are provided, but we need it
#
add_library(tier0 INTERFACE)
find_library(SOURCE_SDK_TIER0 tier0 tier0.lib REQUIRED PATHS
    "${CMAKE_CURRENT_LIST_DIR}/tier0"
)
target_link_libraries(tier0 INTERFACE "${SOURCE_SDK_TIER0}")

#
# Tier1
#
add_library(tier1 STATIC
    "${CMAKE_CURRENT_LIST_DIR}/tier1/checksum_md5.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/generichash.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/memstack.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/rangecheckedvar.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/characterset.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/sparsematrix.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/datamanager.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/stringpool.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/tokenreader.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/kvpacker.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/snappy-stubs-internal.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/interface.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/checksum_sha1.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/utlbufferutil.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/utlstring.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/snappy.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/ilocalize.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/utlbinaryblock.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/convar.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/uniqueid.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/NetAdr.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/commandbuffer.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/bitbuf.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/diff.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/splitstring.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/tier1.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/pathmatch.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/strtools.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/strtools_unicode.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/reliabletimer.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/undiff.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/processor_detect.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/utlbuffer.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/newbitbuf.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/KeyValues.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/fileio.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/utlsymbol.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/qsort_s.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/checksum_crc.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/lzmaDecoder.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/mempool.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/snappy-sinksource.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/tier1/byteswap.cpp"
)
if(UNIX AND NOT APPLE)
    target_sources(tier1 PRIVATE
        "${CMAKE_CURRENT_LIST_DIR}/tier1/processor_detect_linux.cpp"
    )
endif()
target_link_libraries(tier1 PUBLIC source_sdk_meta)
