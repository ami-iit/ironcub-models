
cmake_minimum_required(VERSION 3.22.1)
project(ironcub-models LANGUAGES NONE)
set(BUILD_PREFIX "iRonCub")
include(GNUInstallDirs)

set(GAZEBO_SUPPORTED_MODELS "")
list(APPEND GAZEBO_SUPPORTED_MODELS "iRonCub-Mk3_Gazebo")


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

# Copy the ergoCub folder in the build tree
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/${BUILD_PREFIX} DESTINATION ${CMAKE_BINARY_DIR})

SUBDIRLIST(ROBOTS_NAMES ${CMAKE_CURRENT_SOURCE_DIR}/${BUILD_PREFIX}/robots)
foreach(ROBOT_DIRNAME ${ROBOTS_NAMES})
  set(ROBOT_NAME ${ROBOT_DIRNAME})
  set(ROBOT_MODEL_CONFIG_FILE "${CMAKE_BINARY_DIR}/${BUILD_PREFIX}/robots/${ROBOT_NAME}/model.config")


  list(APPEND INSTALLED_URDF_MODELS \"${ROBOT_NAME}\")
  # When copying in sources remove the _fixed models
  if(ROBOT_NAME IN_LIST GAZEBO_SUPPORTED_MODELS)

    set(ROBOT_NAME_CONFIG "${ROBOT_NAME}")
    set(ROBOT_MODEL_CONFIG "model.urdf")
    set(ROBOT_MODEL_DESCRIPTION_CONFIG "Model for the iRonCub-Mk3 humanoid robot.")
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/model.config.in
                   ${ROBOT_MODEL_CONFIG_FILE}
                   @ONLY)

  endif()
endforeach()

# install the directory
install(DIRECTORY ${CMAKE_BINARY_DIR}/${BUILD_PREFIX} DESTINATION share)
