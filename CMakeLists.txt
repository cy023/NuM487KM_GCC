
cmake_minimum_required(VERSION 3.15.3)

# Path to toolchain file. This one has to be before 'project()' below
set(CMAKE_TOOLCHAIN_FILE ${CMAKE_SOURCE_DIR}/arm-none-eabi-gcc.cmake)
#### PROJECT_NAME, will produce PROJECT_NAME.elf ######################
project(M487KMCAN)
#######################################################################
#### definitions ######################################################
set(EXECUTABLE ${PROJECT_NAME}.elf)
set(LINKER_FILE ${CMAKE_SOURCE_DIR}/Device_Startup/gcc_arm.ld)

set(DEFINE_COMPILE_FLAGS
    -mcpu=cortex-m4
    -mthumb
    -mfpu=fpv4-sp-d16
    -mfloat-abi=hard

    -fdata-sections     # make unused data gc-able
    -ffunction-sections # make unused function gc-able

    -Wall
    -O0
    -g3
)

enable_language(C ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)
##########################################################################
#### include module, like makefile xxx.mk ################################
include(m480bsp.cmake)
# include(freertos.cmake)
##########################################################################
#### add compile-needed file(avoid another CMakeLists.txt) ###############
set(SRC_FILES
    Core/main.c
)
##########################################################################
#### define include directories path #####################################
# set(INCLUDE_DIR )
##########################################################################
#### Build the executable based on the source files ######################
add_executable(${EXECUTABLE} ${SRC_FILES})

target_link_libraries(${EXECUTABLE}
        m480bsp
        # freertos
)
##########################################################################
#### List include directories ############################################
target_include_directories(${EXECUTABLE} PRIVATE
        # ${INCLUDE_DIR}
        m480bsp
        # freertos
)
##########################################################################
#### Compiler options ####################################################
target_compile_options(${EXECUTABLE} PRIVATE
        ${DEFINE_COMPILE_FLAGS}
        )

#### Linker options ######################################################
target_link_options(${EXECUTABLE} PRIVATE
        -T${LINKER_FILE}
        -mcpu=cortex-m4
        -mthumb
        -mfpu=fpv4-sp-d16
        -mfloat-abi=hard
        -specs=nano.specs
        -specs=nosys.specs
        -lc     # C standard library
        -lm     # math library(math.h)
        -Wl,-Map=${PROJECT_NAME}.map,--cref
        -Wl,--gc-sections   # gc no-referenced code/data
        -Xlinker -print-memory-usage -Xlinker
        )

# Optional: Print executable size as part of the post build process
add_custom_command(TARGET ${EXECUTABLE}
        POST_BUILD
        COMMAND ${CMAKE_SIZE_UTIL} ${EXECUTABLE})

# Optional: Create hex, bin and S-Record files after the build
add_custom_command(TARGET ${EXECUTABLE}
        POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} -O ihex ${EXECUTABLE} ${PROJECT_NAME}.hex
        COMMAND ${CMAKE_OBJCOPY} -O binary ${EXECUTABLE} ${PROJECT_NAME}.bin)