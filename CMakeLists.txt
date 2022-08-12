#******************************************************************************
#   Copyright 2020 Embedded Office GmbH & Co. KG
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.
#******************************************************************************

#---
# Preamble
#
cmake_minimum_required(VERSION 3.20)     # buildPresets is introduced in 3.20
project(stm32f7xx-hal)

#---
# Target exported by this project as 'stm32f7xx-hal'
#
# You must configure the HAL with some cached variables in your project:
# - STM_HAL_CONF_DIR        : [path]    directory which holds the configuration: stm32f7xx_hal_conf.h
# - STM_HAL_DEVICE          : [string]  select a supported compiler definition (e.g. 'STM32F76XX')
# - STM_USE_HAL_DRIVER      : [boolean] true = set compiler definition 'USE_HAL_DRIVER'
# - STM_USE_FULL_LL_DRIVER  : [boolean] true = set compiler definition 'USE_FULL_LL_DRIVER'
# - STM_USE_FULL_ASSERT     : [bbolean] true = set compiler definition 'USE_FULL_ASSERT'
add_subdirectory(Drivers)

#---
# This is needed only if we are the top level project
#
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
  
  # Setup target which creates a source package for efficient usage with Cmake CPM/FetchContent
  set(package_files
    Drivers/CMSIS/Device
    Drivers/CMSIS/Include
    Drivers/STM32F7xx_HAL_Driver
    Drivers/CMakeLists.txt
    CMakeLists.txt
    LICENSE.md
  )
  add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-src.zip
    COMMAND ${CMAKE_COMMAND} -E tar c ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-src.zip --format=zip -- ${package_files}
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    DEPENDS ${package_files}
  )
  add_custom_target(${PROJECT_NAME}_package DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-src.zip)
    
endif()
