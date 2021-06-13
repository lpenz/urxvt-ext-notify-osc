# Copyright (C) 2018 Leandro Lisboa Penz <lpenz@lpenz.org>
# This file is subject to the terms and conditions defined in
# file 'LICENSE.txt', which is part of this source code package.

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
    DESTINATION "${CMAKE_INSTALL_PREFIX}/lib/${CMAKE_LIBRARY_ARCHITECTURE}/urxvt/perl"
    )
install (
    FILES README.md LICENSE AUTHORS
    DESTINATION ${CMAKE_INSTALL_DOCDIR}/
    )

