## Pok3rTool CMakeLists.txt
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

PROJECT(Pok3rTool)

ADD_SUBDIRECTORY(libchaos)
ADD_SUBDIRECTORY(rawhid)

SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR})
FIND_PACKAGE(LibUSB-1.0 REQUIRED)

### =================== SOURCES =================== ###

SET(POK3RTOOL_SOURCES
    main.cpp
    pok3r.h
    pok3r.cpp
    pok3r_rgb.h
    pok3r_rgb.cpp

    hiddevice.h
    hiddevice.cpp

    usbdevice.h
    usbdevice.cpp

    updateinterface.h
)

SET(FILES
    99-pok3r.rules
)

### =================== CONFIG ==================== ###

INCLUDE_DIRECTORIES(
    ${LibChaos_INCLUDE}
    ${LIBUSB_1_INCLUDE_DIRS}
)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${LibChaos_CXX_FLAGS} ${LIBUSB_1_DEFINITIONS}")
ADD_DEFINITIONS(-DBUILDING)

### =================== BUILD =================== ###

ADD_CUSTOM_TARGET(pok3rtool-dummy SOURCES ${FILES})

ADD_EXECUTABLE(pok3rtool ${POK3RTOOL_SOURCES})
LibChaos_Configure_Target(pok3rtool)
TARGET_LINK_LIBRARIES(pok3rtool rawhid ${LIBUSB_1_LIBRARIES})
