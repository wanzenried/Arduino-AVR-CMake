cmake_minimum_required(VERSION 3.25)
project(Blink LANGUAGES C CXX ASM)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

include(cmake/lto.cmake)
include(cmake/arduino_core.cmake)
include(cmake/arduino_hex.cmake)
include(cmake/arduino_upload.cmake)
include(cmake/arduino_libs.cmake)

# List all source files used here
set(SOURCES
    src/blink.cpp
)

add_executable(${PROJECT_NAME} ${SOURCES})

# List all include directories here
target_include_directories(${PROJECT_NAME} PRIVATE
    ${CMAKE_SOURCE_DIR}/inc
)

# Remove comments from libraries you wish to use
target_link_libraries(${PROJECT_NAME} PUBLIC 
    ArduinoCore
    #ArduinoEEPROM
    #ArduinoHID
    #ArduinoSPI
    #ArduinoSoftSerial
    #ArduinoWire
)
target_compile_options(${PROJECT_NAME} PRIVATE
    "-Wall"
    "-Wextra"
    "-pedantic"
)
arduino_avr_hex(${PROJECT_NAME})

set(PORT "/dev/ttyACM0"
    CACHE STRING "The serial port for uploading using avrdude")
arduino_avr_upload(${PROJECT_NAME} ${PORT})
