add_library(binlogrouter SHARED blr.c blr_master.c blr_cache.c blr_slave.c blr_file.c)
set_target_properties(binlogrouter PROPERTIES INSTALL_RPATH ${CMAKE_INSTALL_RPATH}:${MAXSCALE_LIBDIR} VERSION "2.0.0")
set_target_properties(binlogrouter PROPERTIES LINK_FLAGS -Wl,-z,defs)
# libbinlogrouter refers to my_uuid_init and my_uuid. They are non-public functions and
# should not be used. They are found only from the embedded lib.
target_link_libraries(binlogrouter maxscale-common ${MYSQL_EMBEDDED_LIBRARIES} ${PCRE_LINK_FLAGS})
install(TARGETS binlogrouter DESTINATION ${MAXSCALE_LIBDIR})

add_executable(maxbinlogcheck maxbinlogcheck.c blr_file.c blr_cache.c blr_master.c blr_slave.c blr.c)
# maxbinlogcheck refers to my_uuid_init and my_uuid. They are non-public functions and
# should not be used. They are found only from the embedded lib.
target_link_libraries(maxbinlogcheck maxscale-common ${MYSQL_EMBEDDED_LIBRARIES} ${PCRE_LINK_FLAGS})

install(TARGETS maxbinlogcheck DESTINATION bin)

if(BUILD_TESTS)
  add_subdirectory(test)
endif()
