CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

PROJECT("RST-sandbox-both-test")

SET(CMAKE_ALLOW_LOOSE_LOOP_CONSTRUCTS TRUE)

FIND_PACKAGE(RST COMPONENTS sandbox)
INCLUDE_DIRECTORIES(BEFORE SYSTEM ${RST_INCLUDE_DIRS} ${RSTSANDBOX_INCLUDE_DIRS})
ADD_DEFINITIONS(${RST_CFLAGS} ${RSTSANDBOX_CFLAGS})

ADD_EXECUTABLE(tester tester.cpp)
TARGET_LINK_LIBRARIES(tester ${RST_LIBRARIES} ${RSTSANDBOX_LIBRARIES})

MESSAGE("RST_STABLE_PROTOS: ${RST_STABLE_PROTOS}")
MESSAGE("RST_SANDBOX_PROTOS: ${RST_SANDBOX_PROTOS}")
