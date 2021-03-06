SET(KIT Lepton)
SET(UKIT LEPTON)

##
## Version stuff
##
SET(OPENSIM_LEPTON_LIBRARY_NAME osim${KIT})
SET(OPENSIM_LEPTON_MAJOR_VERSION 1)
SET(OPENSIM_LEPTON_MINOR_VERSION 1)
SET(OPENSIM_LEPTON_BUILD_VERSION 0)
SET(OPENSIM_LEPTON_COPYRIGHT_YEARS "2005-6")
# comma separated list of authors
SET(OPENSIM_LEPTON_AUTHORS  "Peter_Eastman") 


ADD_DEFINITIONS(-DOPENSIM_LEPTON_LIBRARY_NAME=${OPENSIM_LEPTON_LIBRARY_NAME}
-DOPENSIM_LEPTON_MAJOR_VERSION=${OPENSIM_LEPTON_MAJOR_VERSION}
-DOPENSIM_LEPTON_MINOR_VERSION=${OPENSIM_LEPTON_MINOR_VERSION}
-DOPENSIM_LEPTON_BUILD_VERSION=${OPENSIM_LEPTON_BUILD_VERSION}
-DOPENSIM_LEPTON_COPYRIGHT_YEARS="${OPENSIM_LEPTON_COPYRIGHT_YEARS}"
-DOPENSIM_LEPTON_AUTHORS="${OPENSIM_LEPTON_AUTHORS}"
-DOPENSIM_LEPTON_TYPE="Shared")

FILE(GLOB SOURCE_FILES src/*.cpp)
FILE(GLOB INCLUDE_FILES include/Lepton.h)

INCLUDE_DIRECTORIES(${OpenSim_SOURCE_DIR}/Vendors/lepton/include)

IF(WIN32)
ADD_DEFINITIONS("-DLEPTON_BUILDING_SHARED_LIBRARY")
ENDIF(WIN32)

ADD_LIBRARY(osim${KIT} SHARED ${SOURCE_FILES} ${INCLUDE_FILES}) 

SET_TARGET_PROPERTIES(osim${KIT} PROPERTIES PROJECT_LABEL "Vendor Libraries - osim${KIT}")

# Shared libraries are needed at runtime for applications, so we put them
# at the top level in OpenSim/bin/*.dll (Windows) or OpenSim/lib/*.so (Linux)
# or OpemSim/lib/*.dylib (Mac). Windows .lib files, and Linux/Mac .a static
# archives are only needed at link time so go in sdk/lib.
IF(WIN32)
    INSTALL(TARGETS osim${KIT}
            RUNTIME DESTINATION bin
            LIBRARY DESTINATION sdk/lib
            ARCHIVE DESTINATION sdk/lib)
ELSE()
    INSTALL(TARGETS osim${KIT}
            RUNTIME DESTINATION bin
            LIBRARY DESTINATION lib
            ARCHIVE DESTINATION sdk/lib)
ENDIF()

# To include it in public release. 

INSTALL(FILES ${INCLUDE_FILES}
        DESTINATION sdk/include/Vendors/lepton/include)

# Also need to grab all header files from sub dir /Vendors/lepton/include/lepton/		
FILE(GLOB MORE_HEADERS include/lepton/*.h) 
INSTALL(FILES ${MORE_HEADERS}
	DESTINATION sdk/include/Vendors/lepton/include/lepton)


ENABLE_TESTING()

IF (EXECUTABLE_OUTPUT_PATH)
  SET (TEST_PATH ${EXECUTABLE_OUTPUT_PATH})
ELSE (EXECUTABLE_OUTPUT_PATH)
  SET (TEST_PATH .)
ENDIF (EXECUTABLE_OUTPUT_PATH)
#

SUBDIRS(test)
