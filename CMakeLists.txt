# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# initalize pico_sdk from installed location
# (note this can come from environment, CMake cache etc)
set(PICO_SDK_PATH "/home/nick/pico/pico-sdk")

# Pull in Pico SDK (must be before project)
include(pico_sdk_import.cmake)

project(rtcSet C CXX)

# Initialise the Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

add_executable(rtcSet rtcSet.c )

pico_set_program_name(rtcSet "rtcSet")
pico_set_program_version(rtcSet "0.1")

pico_enable_stdio_uart(rtcSet 1)
pico_enable_stdio_usb(rtcSet 0)

# Add the standard library to the build
target_link_libraries(rtcSet pico_stdlib)

# Add any user requested libraries
target_link_libraries(rtcSet
        hardware_i2c
        )

pico_add_extra_outputs(rtcSet)

