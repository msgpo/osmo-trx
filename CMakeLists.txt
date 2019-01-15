cmake_minimum_required(VERSION 2.8)
project(osmo-trx C CXX)

# Set the version information here
set(MAJOR_VERSION 0)
set(API_COMPAT    0)
set(MINOR_VERSION 1)
set(MAINT_VERSION git)

set(LIBVER  "${MAJOR_VERSION}.${API_COMPAT}.${MINOR_VERSION}")

include_directories(CommonLibs)
include_directories(GSM)

add_definitions(-Wall -g -Dunix -DHAVE_CONFIG_H)

set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} -std=c++11)
set(CMAKE_C_FLAGS ${CMAKE_C_FLAGS} -std=c11)


#set(BUILD_SHARED_LIBS ON)

CONFIGURE_FILE(
    ${CMAKE_CURRENT_SOURCE_DIR}/config.h.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/config.h
@ONLY)

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

find_package(FFTW)
find_package(LibXTRX)

find_library(sqlite3 sqlite3)
#add_subdirectory(sqlite3)


include_directories(${CMAKE_CURRENT_BINARY_DIR})

add_subdirectory(CommonLibs)
add_subdirectory(GSM)
add_subdirectory(Transceiver52M)





