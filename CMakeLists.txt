# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.1.1)
set(toolchainVersion 14_2_Rel1)
set(picotoolVersion 2.1.1)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico CACHE STRING "Board type")

cmake_minimum_required(VERSION 3.12)

include(pico_sdk_import.cmake)
project(capuchin-firmware C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

pico_sdk_init()

add_executable(${PROJECT_NAME})

set(PICO_TINYUSB_PATH ${PICO_SDK_PATH}/lib/tinyusb)

target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR} ${PICO_TINYUSB_PATH})

pico_generate_pio_header(${PROJECT_NAME} ${CMAKE_CURRENT_LIST_DIR}/uart.pio)

target_sources(${PROJECT_NAME} PRIVATE
    main.c
)
target_link_libraries(${PROJECT_NAME}
    pico_stdlib 
    tinyusb_device 
    tinyusb_board 
    pico_stdio 
    hardware_i2c 
    pico_multicore 
    pico_time 
    pico_sync 
    hardware_pio
)
pico_add_extra_outputs(${PROJECT_NAME})
