
add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/admin_lua_proc.h
  COMMAND ${CMAKE_COMMAND} -P ${VALHALLA_SOURCE_DIR}/cmake/Binary2Header.cmake
      ${VALHALLA_SOURCE_DIR}/lua/admin.lua
      ${CMAKE_CURRENT_BINARY_DIR}/admin_lua_proc.h
      --variable-name lua_admin_lua
  WORKING_DIRECTORY ${VALHALLA_SOURCE_DIR}
  COMMENT "Compiling lua/admin.lua to admin_lua_proc.h"
  DEPENDS ${VALHALLA_SOURCE_DIR}/lua/admin.lua
  VERBATIM)

add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/graph_lua_proc.h
  COMMAND ${CMAKE_COMMAND} -P ${VALHALLA_SOURCE_DIR}/cmake/Binary2Header.cmake
      ${VALHALLA_SOURCE_DIR}/lua/graph.lua
      ${CMAKE_CURRENT_BINARY_DIR}/graph_lua_proc.h
      --variable-name lua_graph_lua
  WORKING_DIRECTORY ${VALHALLA_SOURCE_DIR}
  COMMENT "Compiling lua/graph.lua to graph_lua_proc.h"
  DEPENDS ${VALHALLA_SOURCE_DIR}/lua/graph.lua
  VERBATIM)

file(GLOB headers ${VALHALLA_SOURCE_DIR}/valhalla/mjolnir/*.h)

set(sources
  ${CMAKE_CURRENT_BINARY_DIR}/graph_lua_proc.h
  ${CMAKE_CURRENT_BINARY_DIR}/admin_lua_proc.h
  admin.cc
  adminbuilder.cc
  bssbuilder.cc
  complexrestrictionbuilder.cc
  convert_transit.cc
  countryaccess.cc
  dataquality.cc
  directededgebuilder.cc
  edgeinfobuilder.cc
  elevationbuilder.cc
  ferry_connections.cc
  graphbuilder.cc
  graphenhancer.cc
  graphfilter.cc
  graphtilebuilder.cc
  graphvalidator.cc
  hierarchybuilder.cc
  ingest_transit.cc
  linkclassification.cc
  luatagtransform.cc
  node_expander.cc
  osmdata.cc
  osmpbfparser.cc
  osmaccessrestriction.cc
  osmrestriction.cc
  osmway.cc
  pbfadminparser.cc
  pbfgraphparser.cc
  restrictionbuilder.cc
  servicedays.cc
  shortcutbuilder.cc
  speed_assigner.h
  timeparsing.cc
  transitbuilder.cc
  util.cc
  validatetransit.cc)

valhalla_module(NAME mjolnir
  SOURCES ${sources}
  HEADERS ${headers}
  INCLUDE_DIRECTORIES
    PUBLIC
      ${VALHALLA_SOURCE_DIR}
      ${VALHALLA_SOURCE_DIR}/valhalla
      ${VALHALLA_SOURCE_DIR}/third_party/date/include
      ${VALHALLA_SOURCE_DIR}/third_party/rapidjson/include
      $<$<BOOL:${WIN32}>:${VALHALLA_SOURCE_DIR}/third_party/dirent/include>
      ${PROTOBUF_INCLUDE_DIR}
   PRIVATE
      ${CMAKE_CURRENT_BINARY_DIR}
      ${CMAKE_CURRENT_BINARY_DIR}/valhalla
      ${VALHALLA_SOURCE_DIR}/third_party/just_gtfs/include
  DEPENDS
    valhalla::proto
    valhalla::baldr
    SpatiaLite::SpatiaLite
    SQLite3::SQLite3
    Boost::boost
    Lua::Lua
    Threads::Threads
    ZLIB::ZLIB
    robin_hood::robin_hood)
