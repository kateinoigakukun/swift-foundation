##===----------------------------------------------------------------------===##
##
## This source file is part of the Swift open source project
##
## Copyright (c) 2024 Apple Inc. and the Swift project authors
## Licensed under Apache License v2.0
##
## See LICENSE.txt for license information
## See CONTRIBUTORS.md for the list of Swift project authors
##
## SPDX-License-Identifier: Apache-2.0
##
##===----------------------------------------------------------------------===##

include(ExternalProject)
if(CMAKE_HOST_WIN32)
  set(_FoundationMacrosSwiftFlags -DCMAKE_Swift_FLAGS="-use-ld=lld")
endif()
ExternalProject_Add(FoundationMacros
  SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/FoundationMacros"
  PREFIX "${CMAKE_BINARY_DIR}/_deps"
  BINARY_DIR "macros"
  CMAKE_ARGS
    -DCMAKE_MAKE_PROGRAM=${CMAKE_MAKE_PROGRAM}
    -DCMAKE_Swift_COMPILER=${CMAKE_Swift_COMPILER}
    ${_FoundationMacrosSwiftFlags}
  INSTALL_COMMAND "")
ExternalProject_Get_Property(FoundationMacros BINARY_DIR)
if(CMAKE_HOST_WIN32)
  set(_SwiftFoundation_PredicateMacro "${BINARY_DIR}/FoundationMacros.exe#PredicateMacro")
  set(_SwiftFoundation_ExpressionMacro "${BINARY_DIR}/FoundationMacros.exe#ExpressionMacro")
else()
  set(_SwiftFoundation_PredicateMacro "${BINARY_DIR}/FoundationMacros#PredicateMacro")
  set(_SwiftFoundation_ExpressionMacro "${BINARY_DIR}/FoundationMacros#ExpressionMacro")
endif()

add_subdirectory(_FoundationCShims)
add_subdirectory(FoundationEssentials)
add_subdirectory(FoundationInternationalization)
