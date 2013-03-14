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

foreach(lang ${DFF_GUIDE_LANGUAGES})
  list(APPEND QHC_GUIDE_DEPENDENCIES doc.guide.${lang}.qhc)
  list(APPEND QCH_GUIDE_FILES dff_guide_${lang}.qch)
endforeach(lang)

find_program(QHELPGENERATOR_EXECUTABLE qhelpgenerator)
if (QHELPGENERATOR_EXECUTABLE)
  set(DOC_COLLECTION_PROJECT_FILE "dff_doc.qhcp")
  set(DOC_COLLECTION_FILE "dff_doc.qhc")
  foreach (lang ${DFF_GUIDE_LANGUAGES})
    add_custom_target(doc.guide.${lang}.qhc
                      DEPENDS doc.guide.${lang}.html
                      COMMAND ${QHELPGENERATOR_EXECUTABLE} dff_guide_${lang}.qhp -o ../qhc/dff_guide_${lang}.qch 
                      WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../guide
                     )
    set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES dff_guide_${lang}.qch) 
  endforeach(lang)

  find_program(QCOLLECTIONGENERATOR_EXECUTABLE qcollectiongenerator)
  if (QCOLLECTIONGENERATOR_EXECUTABLE)
  add_custom_target(doc.qhc.collection
                    DEPENDS ${QHC_GUIDE_DEPENDENCIES} doc.developer.qch
                    COMMAND ${QCOLLECTIONGENERATOR_EXECUTABLE} ${DOC_COLLECTION_PROJECT_FILE} -o ${DOC_COLLECTION_FILE} 
                    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                   )
  set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES "${DOC_COLLECTION_FILE};${DOC_QCH_OUTPUT_FILE};${QCH_GUIDE_FILES}")
  endif (QCOLLECTIONGENERATOR_EXECUTABLE)
endif (QHELPGENERATOR_EXECUTABLE)

add_dependencies(doc.qhc ${QHC_GUIDE_DEPENDENCIES} doc.qhc.collection)
