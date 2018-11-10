# GcodeFX-SOEM

BUILDING
========

Prerequisites for all platforms
-------------------------------
 * CMake 2.8.0 or later

Windows (Visual Studio)
-----------------------
 * Start a Visual Studio command prompt then:
   * download and copy Simple Open EtherCAT Master Library to SOEM directory
   * `mkdir build`
   * `cd build`
   * `cmake .. -G "NMake Makefiles"`
   * `nmake`

Linux
-----
   * download and copy Simple Open EtherCAT Master Library to SOEM directory
   * `mkdir build`
   * `cd build`
   * `cmake ..`
   * `make`
