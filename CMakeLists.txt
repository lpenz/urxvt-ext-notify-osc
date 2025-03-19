# Copyright (C) 2018 Leandro Lisboa Penz <lpenz@lpenz.org>
# This file is subject to the terms and conditions defined in
# file 'LICENSE', which is part of this source code package.

cmake_minimum_required(VERSION 3.0)

project(urxvt-ext-notify-osc)

include(GNUInstallDirs)

# Manual

add_custom_command(
    OUTPUT urxvt-ext-notify-osc.1.gz
    DEPENDS notify-osc
    COMMAND pod2man
        ${CMAKE_CURRENT_SOURCE_DIR}/notify-osc |
        gzip -9kc >
        ${CMAKE_CURRENT_BINARY_DIR}/urxvt-ext-notify-osc.1.gz
    )
add_custom_target(manual ALL DEPENDS urxvt-ext-notify-osc.1.gz)

# Install

install(
    FILES ${CMAKE_CURRENT_BINARY_DIR}/urxvt-ext-notify-osc.1.gz
    DESTINATION ${CMAKE_INSTALL_MANDIR}/man1/
    )
install(
    PROGRAMS notify-osc
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/${CMAKE_LIBRARY_ARCHITECTURE}/urxvt/perl"
    )
install (
    FILES README.md LICENSE AUTHORS
    DESTINATION ${CMAKE_INSTALL_DOCDIR}/
    )

# cpack deb

set(CPACK_GENERATOR DEB)
set(CPACK_PACKAGE_CONTACT "Leandro Lisboa Penz <lpenz@lpenz.org>")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "urxvt OSC extension for desktop notifications")
set(CPACK_DEBIAN_PACKAGE_DEPENDS "libfile-which-perl:all, rxvt-unicode")
set(CPACK_DEBIAN_PACKAGE_SECTION utils)
set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
execute_process(COMMAND git describe --tags
                WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
                OUTPUT_VARIABLE GIT_DESCRIBE)
string(REGEX REPLACE "^v([0-9.-]+)(-g[0-9a-f]+)?\n$" "\\1"
  CPACK_PACKAGE_VERSION "${GIT_DESCRIBE}")
set(CPACK_PACKAGE_FILE_NAME "urxvt-ext-notify-osc-${CPACK_PACKAGE_VERSION}")
include(CPack)

