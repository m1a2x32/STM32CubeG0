cmake_minimum_required(VERSION 3.22)

cmake_policy(SET CMP0156 NEW)

project(STM32CubeG0_Library)

set(LIBRARY_NAME STM32CubeG0)

set(CMSIS_DIR ${CMAKE_CURRENT_SOURCE_DIR}/Drivers/CMSIS)
message(STATUS "CMSIS Path: ${CMSIS_DIR}")
set(FREERTOS_DIR ${CMAKE_CURRENT_SOURCE_DIR}/Middlewares/Third_Party/FreeRTOS/Source)
message(STATUS "RTOS Path: ${FREERTOS_DIR}")

set(INCLUDE_LIST 
    ${CMSIS_DIR}/Include
    ${CMSIS_DIR}/Device/ST/STM32G0xx/Include
    ${FREERTOS_DIR}/include
    ${FREERTOS_DIR}/CMSIS_RTOS_V2
    ${FREERTOS_DIR}/portable/GCC/ARM_CM0/
    ${CMAKE_CURRENT_SOURCE_DIR}/ConfigFiles
)
# Include for source files
include_directories(${INCLUDE_LIST})

file(GLOB RTOS_SOURCES
    ${FREERTOS_DIR}/*.c
    ${FREERTOS_DIR}/CMSIS_RTOS_V2/*.c
    ${FREERTOS_DIR}/portable/GCC/ARM_CM0/*.c
    ${FREERTOS_DIR}/portable/MemMang/*.c
)
message(STATUS "RTOS Sources: ${RTOS_SOURCES}")

add_library(${LIBRARY_NAME} STATIC
    ${RTOS_SOURCES}
)

target_include_directories(${LIBRARY_NAME} PUBLIC
    ${INCLUDE_LIST}
)

target_compile_options(${LIBRARY_NAME} PRIVATE
    -mthumb
    -mcpu=cortex-m0plus
    -mfloat-abi=soft
)
