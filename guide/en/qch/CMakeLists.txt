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

add_custom_target(doc.guide.en.qch)
add_dependencies(doc.guide.en.html doc.guide.en.qch)

if (BUILD_DFF_GUIDE_QCH)
  find_program(QHELPGENERATOR_EXECUTABLE qhelpgenerator)
  if (QHELPGENERATOR_EXECUTABLE)
    execute_process(COMMAND ${QHELPGENERATOR_EXECUTABLE} dff_guide.qhp -o qch/dff_guide.qch WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../)
    set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES dff_guide.qch) 
 endif (QHELPGENERATOR_EXECUTABLE)
endif(BUILD_DFF_GUIDE_QCH)
