cmake_minimum_required(VERSION 2.6)
project (bncsutil)

# find gmp library
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/")
find_package(GMP REQUIRED)

# test bigendian or littleednian
INCLUDE (TestBigEndian)
TEST_BIG_ENDIAN(BIGENDIAN)
if(BIGENDIAN)
  ADD_DEFINITIONS(-DBIGENDIAN)
else(BIGENDIAN)
  ADD_DEFINITIONS(-DLITTLEENDIAN)
endif(BIGENDIAN)

include_directories( src/ )

# headers
set ( HEADERS
	src/bncsutil/bncsutil.h
	src/bncsutil/bsha1.h
	src/bncsutil/buffer.h
	src/bncsutil/cdkeydecoder.h
	src/bncsutil/checkrevision.h
	src/bncsutil/debug.h
	src/bncsutil/decodekey.h
	src/bncsutil/file.h
	src/bncsutil/keytables.h
	src/bncsutil/libinfo.h
	src/bncsutil/mutil.h
	src/bncsutil/mutil_types.h
	src/bncsutil/nls.h
	src/bncsutil/oldauth.h
	src/bncsutil/pe.h
	src/bncsutil/sha1.h
	src/bncsutil/stack.h
)

# cpp files
set (SOURCE_CPP
	src/bncsutil/bsha1.cpp
	src/bncsutil/cdkeydecoder.cpp
	src/bncsutil/checkrevision.cpp
	src/bncsutil/decodekey.cpp
	src/bncsutil/file.cpp
	src/bncsutil/libinfo.cpp
	src/bncsutil/oldauth.cpp
)

# c files
set (SOURCE_CC 
	src/bncsutil/nls.c
	src/bncsutil/pe.c
	src/bncsutil/sha1.c
	src/bncsutil/stack.c
)
SET_SOURCE_FILES_PROPERTIES( ${SOURCE_CC} PROPERTIES LANGUAGE C )

# TODO debug.c not compiled

# compile options
#add_compile_options( -Wall -Werror )

# create library
add_library( bncsutil SHARED ${SOURCE_CPP} ${SOURCE_CC} )
target_link_libraries( bncsutil ${GMP_LIBRARIES})

set_target_properties( bncsutil PROPERTIES VERSION 1.3.4 SOVERSION 1 )

# install
install( TARGETS bncsutil DESTINATION lib )
install( FILES ${HEADERS} DESTINATION include/bncsutil )
	