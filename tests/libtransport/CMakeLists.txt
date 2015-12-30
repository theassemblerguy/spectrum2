cmake_minimum_required(VERSION 2.6)
FILE(GLOB SRC *.cpp *.h)
FILE(GLOB_RECURSE SWIFTEN_SRC ../include/Swiften/*.cpp)

# Build without openssl on msvc
if (NOT MSVC)
if (APPLE)
	string(REGEX REPLACE "[^;]+;?/Schannel/[^;]+;?" "" SWIFTEN_SRC "${SWIFTEN_SRC}")
	string(REGEX REPLACE "[^;]+;?/OpenSSL/[^;]+;?" "" SWIFTEN_SRC "${SWIFTEN_SRC}")
else()
	string(REGEX REPLACE "[^;]+;?/Schannel/[^;]+;?" "" SWIFTEN_SRC "${SWIFTEN_SRC}")
	string(REGEX REPLACE "[^;]+;?/SecureTransport/[^;]+;?" "" SWIFTEN_SRC "${SWIFTEN_SRC}") 
endif()
else()
	string(REGEX REPLACE "[^;]+;?/OpenSSL/[^;]+;?" "" SWIFTEN_SRC "${SWIFTEN_SRC}") 
	string(REGEX REPLACE "[^;]+;?/SecureTransport/[^;]+;?" "" SWIFTEN_SRC "${SWIFTEN_SRC}")
endif()

FILE(GLOB HEADERS ../../include/transport/*.h)
include_directories(../../spectrum/src/frontends/xmpp/)

if (CPPUNIT_FOUND)
	FILE(GLOB SRC_TEST *.cpp)
	FILE(GLOB SRC_TEST_FRONTEND ../../spectrum/src/frontends/xmpp/*.cpp)

	ADD_EXECUTABLE(libtransport_test ${SRC_TEST} ${SRC_TEST_FRONTEND})
	set_target_properties(libtransport_test PROPERTIES COMPILE_DEFINITIONS LIBTRANSPORT_TEST=1)
	

	target_link_libraries(libtransport_test transport ${CPPUNIT_LIBRARY} ${Boost_LIBRARIES})
endif()