# Set minimum required version of CMake
cmake_minimum_required(VERSION 3.12)

# Include build functions from Pico SDK
include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

# Set name of project (as PROJECT_NAME) and C/C   standards
project(diff_robot C CXX ASM) # Change this name *blink*
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Creates a pico-sdk subdirectory in our project for the libraries
pico_sdk_init()

# NRF24 libraries
include(lib/RF24/CMakeLists.txt)
include(lib/RF24Network/CMakeLists.txt)

# Tell CMake where to find the executable source file
add_executable(${PROJECT_NAME}
    main.cpp
)

pico_generate_pio_header(${PROJECT_NAME} ${CMAKE_CURRENT_LIST_DIR}/quadrature_encoder.pio)

# Create map/bin/hex/uf2 files
pico_add_extra_outputs(${PROJECT_NAME})

# Link to pico_stdlib (gpio, time, etc. functions)
target_link_libraries(${PROJECT_NAME}
    RF24
    RF24Network
    pico_stdlib
    pico_time
    hardware_adc
    hardware_spi
    hardware_gpio
    hardware_pwm
    pico_multicore
    hardware_pio
)

# Enable usb output, disable uart output
pico_enable_stdio_usb(${PROJECT_NAME} 1)
pico_enable_stdio_uart(${PROJECT_NAME} 0)