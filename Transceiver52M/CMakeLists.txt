#
# Copyright 2008 Free Software Foundation, Inc.
# Copyright 2010 Range Networks, Inc.
#
# This software is distributed under the terms of the GNU Public License.
# See the COPYING file in the main directory for details.
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.
#

include_directories(arch/common)

set(libarch_files
    arch/common/convolve_base.c
    arch/common/convert_base.c
    arch/x86/convert.c
    arch/x86/convolve.c)
add_library(arch ${libarch_files})


include_directories(.)
include_directories(device)


set(COMMON_FILES
	radioInterface.cpp
	radioVector.cpp
	radioClock.cpp
	radioBuffer.cpp
	sigProcLib.cpp
	signalVector.cpp
	Transceiver.cpp
	ChannelizerBase.cpp
	Channelizer.cpp
	Synthesis.cpp
	arch/common/fft.c)

set(libtransceiver_files
	${COMMON_FILES}
	Resampler.cpp
	radioInterfaceResamp.cpp
	radioInterfaceMulti.cpp)

#bin_PROGRAMS = osmo-trx

set(noinst_HEADERS
	Complex.h
	radioInterface.h
	radioVector.h
	radioClock.h
	radioDevice.h
	radioBuffer.h
	sigProcLib.h
	signalVector.h
	Transceiver.h
	USRPDevice.h
	Resampler.h
	ChannelizerBase.h
	Channelizer.h
	Synthesis.h
	common/convolve.h
	common/convert.h
	common/scale.h
	common/mult.h
	common/fft.h)

add_library(transceiver ${libtransceiver_files})

set(LIBOSMOCORE_LIBS osmocore)
set(LIBOSMOCTRL_LIBS osmoctrl)
set(LIBOSMOVTY_LIBS osmovty)

set(DEVICE        device/xtrx/XTRXDevice.cpp)
set(DEVICE_LIBS   ${LIBXTRX_LIBRARIES})
set(DEVICE_INC    ${LIBXTRX_INCLUDE_DIRS})

include_directories(${DEVICE_INC})
add_executable(osmo-trx-xtrx osmo-trx.cpp ${DEVICE})
target_link_libraries(osmo-trx-xtrx
			transceiver
			arch
			GSM
			common
			${FFTW_LIBRARIES}
			${LIBOSMOCORE_LIBS}
			${LIBOSMOCTRL_LIBS}
			${LIBOSMOVTY_LIBS}
			${DEVICE_LIBS}
			pthread dl talloc)


