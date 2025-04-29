project(xxHash)
add_library(xxHash STATIC)

target_include_directories(xxHash 
PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
)

set(XXHASH_SOURCES
    xxhash.h
    xxhash.c
    xxh3.h
    xxh_x86dispatch.h
    xxh_x86dispatch.c
)

if(WIN32)
    target_compile_definitions(xxHash PRIVATE _CRT_SECURE_NO_WARNINGS)
elseif(UNIX AND NOT WIN32)
    set(CMAKE_POSITION_INDEPENDENT_CODE ON)
endif()

target_sources(xxHash PRIVATE ${XXHASH_SOURCES})

target_compile_definitions(xxHash PRIVATE 
    $<$<CONFIG:Debug>:xxHash_DEBUG>
    $<$<CONFIG:Release>:xxHash_RELEASE>
    $<$<CONFIG:Dist>:xxHash_DIST>
)

target_compile_options(xxHash PRIVATE 
    $<$<CONFIG:Debug>:-g>
    $<$<CONFIG:Release>:-O2>
    $<$<CONFIG:Dist>:-O2 -s>
)
