# DFF -- An Open Source Digital Forensics Framework
# Copyright (C) 2009-2013 ArxSys
# This program is free software, distributed under the terms of
# the GNU General Public License Version 2. See the LICENSE file
# at the top of the source tree.
#  
# See http://www.digital-forensic.org for more information about this
# project. Please do not directly contact any of the maintainers of
# DFF for assistance; the project provides a web site, mailing lists
# and IRC channels for your use.
# 
# Author(s):
#  Solal Jacob <sja@digital-forensic.org>

add_custom_target(doc.developer)
add_dependencies(doc doc.developer)

set (SOURCES
 "${CMAKE_SOURCE_DIR}/dff/api/include/vfs.hpp"
 "${CMAKE_SOURCE_DIR}/dff/api/include/node.hpp"
 "${CMAKE_SOURCE_DIR}/dff/api/include/filemapping.hpp"
 "${CMAKE_SOURCE_DIR}/dff/api/include/fso.hpp"
 "${CMAKE_SOURCE_DIR}/dff/api/include/mfso.hpp"
 "${CMAKE_SOURCE_DIR}/dff/api/vfs/vfs.cpp"
 "${CMAKE_SOURCE_DIR}/dff/api/vfs/node.cpp"
 "${CMAKE_SOURCE_DIR}/dff/api/vfs/filemapping.cpp"
 "${CMAKE_SOURCE_DIR}/dff/api/vfs/fso.cpp"
 "${CMAKE_SOURCE_DIR}/dff/api/vfs/mfso.cpp"
 "${CMAKE_SOURCE_DIR}/doc/developer/dff.coverpage"
)

find_package(Doxygen)
if (DOXYGEN_FOUND)
##XXX
  set (DOC_OUTPUT_TYPES HTML QCH) #LATEX

  string (REPLACE ";" " " DOC_SOURCE_FILES "${SOURCES}")
  set (DOC_VERSION "${CPACK_PACKAGE_VERSION_MAJOR}.${CPACK_PACKAGE_VERSION_MINOR}.${CPACK_PACKAGE_VERSION_PATCH}")
  set (DOC_BINARY_DIR ${CMAKE_CURRENT_BINARY_DIR})
  set (DOC_LOGO_FILE ${CMAKE_SOURCE_DIR}/"ressources/dff.png")
  set (DOC_HTML_OUTPUT_FILE "index.html")
  set (DOC_QCH_OUTPUT_FILE "dff_developer.qch")
  set (DOC_LATEX_OUTPUT_FILE "refman.tex")

  file(MAKE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/qch) 
  foreach (type ${DOC_OUTPUT_TYPES})
    set (DOC_${type}_OUTPUT "YES")
    string (TOLOWER "${type}" type_low)
    set (DOC_${type}_OUTPUT_FILE_ABS "${CMAKE_CURRENT_BINARY_DIR}/${type_low}/${DOC_${type}_OUTPUT_FILE}")
    add_custom_target (doc.developer.${type_low} DEPENDS ${DOC_${type}_OUTPUT_FILE_ABS})
    add_dependencies (doc.developer doc.developer.${type_low})
    list (APPEND DOC_OUTPUT_FILES_ABS "${DOC_${type}_OUTPUT_FILE_ABS}")
  endforeach (type)
  configure_file (
      ${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.in
      ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile
      @ONLY
    )

  add_custom_command (
      OUTPUT ${DOC_OUTPUT_FILES_ABS}
      COMMAND "${DOXYGEN_EXECUTABLE}" "${CMAKE_CURRENT_BINARY_DIR}/Doxyfile"
      DEPENDS "${CMAKE_CURRENT_BINARY_DIR}/Doxyfile" ${SOURCES}
      VERBATIM
  )

  set_directory_properties (PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES "html;qch/dff_developer.qch;Doxyfile")

  install (DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/html"
    DESTINATION ${DOCUMENTATION_INSTALL_DIR}
    COMPONENT documentation
  )
endif (DOXYGEN_FOUND)
