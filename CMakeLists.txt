cmake_minimum_required(VERSION 2.8.12)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules")
project(GCFXSOEM C)

if(WIN32)
  set(OS "win32")
  include_directories(SOEM/oshw/win32/wpcap/Include)
  if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    link_directories(${CMAKE_SOURCE_DIR}/SOEM/oshw/win32/wpcap/Lib/x64)
  elseif(CMAKE_SIZEOF_VOID_P EQUAL 4)
    link_directories(${CMAKE_SOURCE_DIR}/SOEM/oshw/win32/wpcap/Lib)
  endif()
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /D _CRT_SECURE_NO_WARNINGS")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  /WX")
  set(OS_LIBS soem.lib wpcap.lib Packet.lib Ws2_32.lib Winmm.lib)
  file(GLOB GCFX_DEF gcodefx/gcfxsoem.def)
elseif(UNIX AND NOT APPLE)
  set(OS "linux")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra -Werror")
  set(OS_LIBS soem pthread rt)
  file(GLOB GCFX_DEF gcodefx/gcfxsoem.map)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wl,--version-script=${GCFX_DEF}")
elseif(APPLE)
  set(OS "macosx")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra -Werror")
  set(OS_LIBS soem pthread pcap)
  file(GLOB GCFX_DEF gcodefx/gcfxsoem.map)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wl,--version-script=${GCFX_DEF}")
endif()

message("OS is ${OS}")

link_directories(${CMAKE_SOURCE_DIR}/SOEM/build)
file(GLOB GCFX_SOURCES gcodefx/*.c)

add_library(gcfxsoem SHARED
  ${GCFX_SOURCES}
  ${GCFX_DEF})
target_link_libraries(gcfxsoem ${OS_LIBS})

target_include_directories(gcfxsoem PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/SOEM/soem>
  $<INSTALL_INTERFACE:include/gcfxsoem>)

target_include_directories(gcfxsoem PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/SOEM/osal>
  $<INSTALL_INTERFACE:include/gcfxsoem>)

target_include_directories(gcfxsoem PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/SOEM/osal/${OS}>
  $<INSTALL_INTERFACE:include/gcfxsoem>)

target_include_directories(gcfxsoem PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/SOEM/oshw/${OS}>
  $<INSTALL_INTERFACE:include/gcfxsoem>)
