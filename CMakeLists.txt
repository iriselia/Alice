project( ALICE )
cmake_minimum_required( VERSION 2.8 )

SET( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${ALICE_SOURCE_DIR}/bin CACHE PATH "Single Directory for all executables." )
SET( CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${ALICE_SOURCE_DIR}/bin_debug CACHE PATH "Single Directory for all executables." )
SET( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${ALICE_SOURCE_DIR}/lib CACHE PATH "Single Directory for all static libraries." )
SET( CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${ALICE_SOURCE_DIR}/lib CACHE PATH "Single Directory for all static libraries." )
SET( CMAKE_PDB_OUTPUT_DIRECTORY ${ALICE_SOURCE_DIR}/bin_debug/pdb CACHE PATH "Single Directory for all static libraries." )
SET( CMAKE_INCLUDE_CURRENT_DIR ON )

file(GLOB_RECURSE ALICE_SOURCES ${ALICE_SOURCE_DIR}/alice/src/*.cpp)
file(GLOB_RECURSE ALICE_HEADERS ${ALICE_SOURCE_DIR}/alice/include/*.h)

set (ALICE_INCLUDE_DIRS "")
foreach (_headerFile ${ALICE_HEADERS})
    get_filename_component(_dir ${_headerFile} PATH)
    list (APPEND ALICE_INCLUDE_DIRS ${_dir})
endforeach()
list(REMOVE_DUPLICATES ALICE_INCLUDE_DIRS)
include_directories( ${ALICE_INCLUDE_DIRS} )

#------ additional includes ------
include_directories(
${DIRECTX_SDK_DIR}/include
${ALICE_SOURCE_DIR}/3rdParty/allegro/include
)

#--- 3rd Party library ---

#------ link directories listed ------
link_directories( ${DIRECTX_SDK_DIR}/lib ${DIRECTX_SDK_DIR}/lib/x64 ${DIRECTX_SDK_DIR}/lib/x86 )
link_directories( ${ALICE_SOURCE_DIR}/lib )
link_libraries( allegro-4.4.2-md-debug )
link_libraries( allegro-4.4.2-md )
#------ target -----
add_executable (ALICE WIN32 ${ALICE_SOURCES} ${ALICE_HEADERS})

if( MSVC )
    SET_TARGET_PROPERTIES( ALICE PROPERTIES LINK_FLAGS "/NODEFAULTLIB:libci" )
endif()
