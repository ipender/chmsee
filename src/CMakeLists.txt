include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_BINARY_DIR}
  ${CHMSEE_INCLUDE_DIRS}
  )
add_definitions(
  ${CHMSEE_CFLAGS_OTHER}
  )
add_custom_command(
  OUTPUT marshal.h
  COMMAND ${GLIB_GENMARSHAL} --prefix=marshal ${CMAKE_CURRENT_SOURCE_DIR}/marshal.list --header > marshal.h
  DEPENDS marshal.list
  )

add_custom_command(
  OUTPUT marshal.c
  COMMAND ${GLIB_GENMARSHAL} --prefix=marshal ${CMAKE_CURRENT_SOURCE_DIR}/marshal.list --body > marshal.c
  DEPENDS marshal.list
  )

link_directories(
  ${CHMSEE_LIBRARY_DIRS}
  )

set(CHMSEE_SRCS
  booktree.c chmsee.c html.c
  setup.c startup.c ui_bookmarks.c booktree.h chmsee.h
  html.h setup.h startup.h ui_bookmarks.h
  marshal.h marshal.c
  ihtml.h ihtml.c
  html-factory.h html-factory.c
  ui_index.h ui_index.c
  ui_chmfile.h ui_chmfile.c
  )
set(CHMSEE_SRCS
  ${CHMSEE_SRCS}
  utils/utils.h utils/utils.c)
set(CHMSEE_SRCS
  ${CHMSEE_SRCS}
  models/bookmarks.h models/bookmarks.c
  models/chmfile.h models/chmfile.c
  models/hhc.h models/hhc.c
  models/link.h models/link.c
  models/ichmfile.h models/ichmfile.c
  models/chmfile-factory.h models/chmfile-factory.c
  models/chmindex.h models/chmindex.c
  )

add_library(chmseelib STATIC ${CHMSEE_SRCS})
target_link_libraries(chmseelib
  ${CHMSEE_LIBRARIES}
  )

add_executable(chmsee main.c)
target_link_libraries(chmsee
  chmseelib
  )
  
install(TARGETS chmsee DESTINATION bin)
