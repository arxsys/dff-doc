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

if (BUILD_DFF_GUIDE_HTML)
  find_package(Docbook)
  include (${Docbook_USE_FILE})
  include (UseDocbook)
  set (DFF_GUIDE_MAIN_XML_FILE "${CMAKE_CURRENT_SOURCE_DIR}/../xml/main.xml")
  docbook_generate (html "${DFF_GUIDE_MAIN_XML_FILE}" XSLT_FILES_HTML)
  foreach (file ${XSLT_FILES_HTML})
    list (APPEND DFF_GUIDE_FILES ${file})
    install (
        FILES ${CMAKE_CURRENT_BINARY_DIR}/${file}
	DESTINATION ${DOCUMENTATION_INSTALL_DIR}
	PERMISSIONS OWNER_READ OWNER_WRITE
                  GROUP_READ
                  WORLD_READ
        COMPONENT documentation
        OPTIONAL
        )
  endforeach ( file )
endif (BUILD_DFF_GUIDE_HTML)

if (DFF_GUIDE_FILES)
  set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES "*.html")
  add_custom_target(doc.guide.en.html DEPENDS ${DFF_GUIDE_FILES})
  add_dependencies(doc.guide.en doc.guide.en.html)
endif (DFF_GUIDE_FILES)
