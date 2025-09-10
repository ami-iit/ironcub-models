
cmake_minimum_required(VERSION 3.22.1)
project(ironcub-models
  VERSION 0.0.2
  LANGUAGES NONE)
set(BUILD_PREFIX "iRonCub-Mk3")
include(GNUInstallDirs)

set(INSTALLED_URDF_MODELS "")

macro(SUBDIRLIST result curdir)
  file(GLOB children RELATIVE ${curdir} ${curdir}/*)
  set(dirlist "")
  foreach(child ${children})
    if(IS_DIRECTORY ${curdir}/${child})
      list(APPEND dirlist ${child})
    endif()
  endforeach()
  set(${result} ${dirlist})
endmacro()

# Copy the iRonCub folder in the build tree
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/${BUILD_PREFIX} DESTINATION ${CMAKE_BINARY_DIR})

SUBDIRLIST(ROBOTS_NAMES ${CMAKE_CURRENT_SOURCE_DIR}/${BUILD_PREFIX}/robots)
foreach(ROBOT_DIRNAME ${ROBOTS_NAMES})
  set(ROBOT_NAME ${ROBOT_DIRNAME})
  set(ROBOT_MODEL_CONFIG_FILE "${CMAKE_BINARY_DIR}/${BUILD_PREFIX}/robots/${ROBOT_NAME}/model.config")


  list(APPEND INSTALLED_URDF_MODELS \"${ROBOT_NAME}\")
endforeach()

# install the directory
install(DIRECTORY ${CMAKE_BINARY_DIR}/${BUILD_PREFIX} DESTINATION share)
