
set(CMAKE_CXX_COMPILE_FLAGS ${CMAKE_CXX_COMPILE_FLAGS} ${MPI_CXX_COMPILE_FLAGS})
set(CMAKE_CXX_LINK_FLAGS ${CMAKE_CXX_LINK_FLAGS} ${MPI_CXX_LINK_FLAGS})

add_definitions(-std=c++11)

set(BUILD_SHARED_LIBS OFF)

# Classes, structures, and constants common to all files
set(COMMON_SRC commonLib.cpp tiffIO.cpp ReadOutlets.cpp)

if(${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION}.${CMAKE_PATCH_VERSION} GREATER 2.8.7)
    # Don't recompile common files by using CMake's 2.8.8 OBJECT "libraries"
    add_library(common_lib OBJECT ${COMMON_SRC})
    set(COMMON_SRC $<TARGET_OBJECTS:common_lib>)
endif()

# MPI is required
find_package(MPI REQUIRED)
include_directories(${MPI_INCLUDE_PATH})
set(CMAKE_CXX_FLAG "${CMAKE_CXX_FLAG} ${MPI_COMPILE_FLAGS}")
set(CMAKE_CXX_LINK_FLAGS "${CMAKE_CXX_LINK_FLAGS} ${MPI_LINK_FLAGS}")

# GDAL is required
find_package(GDAL REQUIRED)
include_directories(${GDAL_INCLUDE_DIR})

# add_executable(aread8 aread8mn.cpp aread8.cpp ${COMMON_SRC})
# add_executable(areadinf areadinfmn.cpp areadinf.cpp ${COMMON_SRC})

add_executable(accd8flowdir D8FlowDirmn.cpp d8.cpp ${COMMON_SRC})
# add_executable(d8flowpathextremeup D8flowpathextremeup.cpp D8FlowPathExtremeUpmn.cpp ${COMMON_SRC})
# add_executable(d8hdisttostrm D8HDistToStrm.cpp D8HDistToStrmmn.cpp ${COMMON_SRC})

# add_executable(dinfavalanche DinfAvalanche.cpp DinfAvalanchemn.cpp ${COMMON_SRC})
# add_executable(dinfconclimaccum DinfConcLimAccum.cpp DinfConcLimAccummn.cpp ${COMMON_SRC})
# add_executable(dinfdecayaccum dinfdecayaccum.cpp DinfDecayAccummn.cpp ${COMMON_SRC})
# add_executable(dinfdistdown DinfDistDown.cpp DinfDistDownmn.cpp ${COMMON_SRC})
# add_executable(dinfdistup DinfDistUp.cpp DinfDistUpmn.cpp ${COMMON_SRC})
add_executable(accdinfflowdir DinfFlowDirmn.cpp dinf.cpp ${COMMON_SRC})
# add_executable(dinfrevaccum DinfRevAccum.cpp DinfRevAccummn.cpp ${COMMON_SRC})
# add_executable(dinftranslimaccum DinfTransLimAccum.cpp DinfTransLimAccummn.cpp ${COMMON_SRC})
# add_executable(dinfupdependence DinfUpDependence.cpp DinfUpDependencemn.cpp ${COMMON_SRC})

# add_executable(dropanalysis DropAnalysis.cpp DropAnalysismn.cpp ${COMMON_SRC})
# add_executable(gridnet gridnetmn.cpp gridnet.cpp ${COMMON_SRC})
# add_executable(lengtharea LengthArea.cpp LengthAreamn.cpp ${COMMON_SRC})
# add_executable(moveoutletstostrm MoveOutletsToStrm.cpp MoveOutletsToStrmmn.cpp ${COMMON_SRC})
# add_executable(peukerdouglas PeukerDouglas.cpp PeukerDouglasmn.cpp ${COMMON_SRC})
# add_executable(pitremove flood.cpp PitRemovemn.cpp ${COMMON_SRC})

# add_executable(slopearea SlopeArea.cpp SlopeAreamn.cpp ${COMMON_SRC})
# add_executable(slopearearatio SlopeAreaRatio.cpp SlopeAreaRatiomn.cpp ${COMMON_SRC})
# add_executable(slopeavedown SlopeAveDown.cpp SlopeAveDownmn.cpp ${COMMON_SRC})
# add_executable(streamnet streamnetmn.cpp streamnet.cpp ${COMMON_SRC})
# add_executable(threshold Threshold.cpp Thresholdmn.cpp ${COMMON_SRC})

# add_executable(gagewatershed gagewatershedmn.cpp gagewatershed.cpp ${COMMON_SRC})


set (MY_TARGETS accd8flowdir accdinfflowdir)

foreach( c_target ${MY_TARGETS} )
    target_link_libraries(${c_target} ${MPI_LIBRARIES} ${GDAL_LIBRARY})
    install(TARGETS ${c_target} DESTINATION bin)
endforeach( c_target ${MY_TARGETS} )