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

find_package (Docbook)
if (Docbook_FOUND)
  include (${Docbook_USE_FILE})
  option (BUILD_DFF_GUIDE_HTML "Build HTML application guide" ON)
  option (BUILD_DFF_GUIDE_QCH  "Build QCH application guide" ON)
  add_custom_target(doc.guide)
  add_dependencies(doc.guide doc)
  add_subdirectory(en)
endif (Docbook_FOUND)


