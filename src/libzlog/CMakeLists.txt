set(libzlog_sources
  log_impl.cc
  stream.cc
  striper.cc
  aio.cc
  capi.cc
  log.cc
  backend.cc
  ../port/stack_trace.cc)

add_definitions("-DZLOG_LIBDIR=\"${CMAKE_INSTALL_FULL_LIBDIR}\"")
add_definitions("-DCMAKE_SHARED_LIBRARY_SUFFIX=\"${CMAKE_SHARED_LIBRARY_SUFFIX}\"")
add_definitions("-DCMAKE_SHARED_LIBRARY_PREFIX=\"${CMAKE_SHARED_LIBRARY_PREFIX}\"")

add_library(libzlog SHARED ${libzlog_sources})

target_link_libraries(libzlog
    zlog_proto
    zlog_seqr
    dl
    ${Boost_SYSTEM_LIBRARY}
    ${Backtrace_LIBRARIES}
)

set_target_properties(libzlog PROPERTIES
  OUTPUT_NAME zlog
  VERSION 1.0.0
  SOVERSION 1)

install(TARGETS libzlog LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})

add_library(test_libzlog OBJECT test_libzlog.cc)
target_include_directories(test_libzlog
  PRIVATE $<TARGET_PROPERTY:gtest,INTERFACE_INCLUDE_DIRECTORIES>)
