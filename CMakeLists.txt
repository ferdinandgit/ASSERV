cmake_minimum_required(VERSION 3.13)
set(CMAKE_VERBOSE_MAKEFILE OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
include(FetchContent)
FetchContent_Declare(
	pico-sdk
	GIT_REPOSITORY https://github.com/raspberrypi/pico-sdk.git
	GIT_SUBMODULES_RECURSE OFF)
FetchContent_MakeAvailable(pico-sdk)

include(${pico-sdk_SOURCE_DIR}/external/pico_sdk_import.cmake)
pico_sdk_init()

project(TechTheTaste LANGUAGES C CXX ASM)

add_executable(main main.c)

add_library (Nyx PID.c encoder.c motion.c motor.c)
add_subdirectory(header)
target_link_libraries( Nyx PUBLIC pico_stdlib hardware_pwm hardware_irq hardware_gpio pico_time )  

target_link_libraries(main PUBLIC Nyx)
target_link_libraries(main PUBLIC pico_stdlib hardware_pwm hardware_irq hardware_gpio pico_time )  


pico_enable_stdio_usb(main ON)
pico_enable_stdio_uart(main ON)

pico_add_extra_outputs(main)

