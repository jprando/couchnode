IF(LCB_INSTALL_HEADERS)
  INSTALL(
      FILES
      libev_io_opts.h
      DESTINATION
      include/libcouchbase)
ENDIF(LCB_INSTALL_HEADERS)

IF(NOT (HAVE_LIBEV3 OR HAVE_LIBEV4))
    RETURN()
ENDIF()


ADD_LIBRARY(couchbase_libev SHARED plugin-libev.c)
SET_TARGET_PROPERTIES(couchbase_libev
    PROPERTIES
    PREFIX "lib"
    IMPORT_PREFIX "lib"
    COMPILE_FLAGS "${CMAKE_C_FLAGS} ${CMAKE_CORE_CFLAGS} -std=gnu99")
TARGET_LINK_LIBRARIES(couchbase_libev ${LIBEV_LIBRARIES})
INCLUDE_DIRECTORIES(AFTER ${LIBEV_INCLUDE_DIR})

IF(HAVE_LIBEV4)
    ADD_DEFINITIONS(-DHAVE_LIBEV4)
ELSE()
    ADD_DEFINITIONS(-DHAVE_LIBEV3)
ENDIF()

INSTALL(TARGETS
    couchbase_libev
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
