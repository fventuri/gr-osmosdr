# Copyright 2012 Free Software Foundation, Inc.
#
# This file is part of gr-osmosdr
#
# gr-osmosdr is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3, or (at your option)
# any later version.
#
# gr-osmosdr is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with gr-osmosdr; see the file COPYING.  If not, write to
# the Free Software Foundation, Inc., 51 Franklin Street,
# Boston, MA 02110-1301, USA.

########################################################################
# This file included, use CMake directory variables
########################################################################

target_include_directories(gnuradio-osmosdr PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${LIBSDRPLAY_INCLUDE_DIRS}
)

APPEND_LIB_LIST(
    ${LIBSDRPLAY_LIBRARIES}
)

list(APPEND gr_osmosdr_srcs
    ${CMAKE_CURRENT_SOURCE_DIR}/sdrplay_source_c.cc
)
set(gr_osmosdr_srcs ${gr_osmosdr_srcs} PARENT_SCOPE)

# Configurable feature set
set(SAMPLE_NUM_GAP_WARNING OFF CACHE BOOL "Display a warning whenever there is a gap in the sample numbers in the stream callback function")
set(ONLY_CHECK_SAMPLE_GAPS OFF CACHE BOOL "Do not send samples back to the client app; just check for gaps in sample numbers")
set(SHOW_GAIN_CHANGES OFF CACHE BOOL "logs an INFO level message whenever a gain change event occurs (very noisy when AGC is enabled)")

if(SAMPLE_NUM_GAP_WARNING)
    target_compile_definitions(gnuradio-osmosdr PRIVATE -DSAMPLE_NUM_GAP_WARNING=1)
endif()
if(ONLY_CHECK_SAMPLE_GAPS)
    target_compile_definitions(gnuradio-osmosdr PRIVATE -DONLY_CHECK_SAMPLE_GAPS=1)
endif()
if(SHOW_GAIN_CHANGES)
    target_compile_definitions(gnuradio-osmosdr PRIVATE -DSHOW_GAIN_CHANGES=1)
endif()
