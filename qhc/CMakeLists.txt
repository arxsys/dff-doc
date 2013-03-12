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

add_custom_target(doc.qhc)
add_dependencies(doc doc.qhc)

find_program(QHELPGENERATOR_EXECUTABLE qhelpgenerator)
if (QHELPGENERATOR_EXECUTABLE)
    add_custom_target(doc.guide.en.qhc
                      DEPENDS doc.guide.en.html
                      COMMAND ${QHELPGENERATOR_EXECUTABLE} dff_guide_en.qhp -o ../../qhc/dff_guide_en.qch 
                      WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../guide/en
                     )
    set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES dff_guide_en.qch) 

    find_program(QCOLLECTIONGENERATOR_EXECUTABLE qcollectiongenerator)
    if (QCOLLECTIONGENERATOR_EXECUTABLE)
    add_custom_target(doc.qhc.collection
                      DEPENDS doc.guide.en.qhc doc.developer.qch
                      COMMAND ${QCOLLECTIONGENERATOR_EXECUTABLE} dff_doc.qhcp -o dff_doc.qhc 
                      WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                     )
    set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES dff_doc.qhc)
    endif (QCOLLECTIONGENERATOR_EXECUTABLE)
endif (QHELPGENERATOR_EXECUTABLE)

add_dependencies(doc.qhc doc.guide.en.qhc doc.qhc.collection)
