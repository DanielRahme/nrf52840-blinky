# CMakeLists.txt for external library

set(SDK_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/nrf5-sdk)

find_file(LINKER_SCRIPT blinky_gcc_nrf52.ld
    PATHS ${SDK_ROOT}/examples/peripheral/blinky/pca10056/blank/armgcc/
)

message("Current source dir from /external")
message(${SDK_ROOT})

set(SRCS_COMMON
  ${SDK_ROOT}/modules/nrfx/mdk/gcc_startup_nrf52840.S 
  ${SDK_ROOT}/components/libraries/log/src/nrf_log_frontend.c 
  ${SDK_ROOT}/components/libraries/log/src/nrf_log_str_formatter.c 
  ${SDK_ROOT}/components/boards/boards.c 
  ${SDK_ROOT}/components/libraries/util/app_error.c 
  ${SDK_ROOT}/components/libraries/util/app_error_handler_gcc.c 
  ${SDK_ROOT}/components/libraries/util/app_error_weak.c 
  ${SDK_ROOT}/components/libraries/util/app_util_platform.c 
  ${SDK_ROOT}/components/libraries/util/nrf_assert.c 
  ${SDK_ROOT}/components/libraries/atomic/nrf_atomic.c 
  ${SDK_ROOT}/components/libraries/balloc/nrf_balloc.c 
  ${SDK_ROOT}/external/fprintf/nrf_fprintf.c 
  ${SDK_ROOT}/external/fprintf/nrf_fprintf_format.c 
  ${SDK_ROOT}/components/libraries/memobj/nrf_memobj.c 
  ${SDK_ROOT}/components/libraries/ringbuf/nrf_ringbuf.c 
  ${SDK_ROOT}/components/libraries/strerror/nrf_strerror.c 
  ${SDK_ROOT}/modules/nrfx/soc/nrfx_atomic.c 
  ${SDK_ROOT}/modules/nrfx/mdk/system_nrf52840.c 
)

set(INCS_COMMON
  ${SDK_ROOT}/components 
  ${SDK_ROOT}/modules/nrfx/mdk 
  #{(PROJ_DI}) 
  ${SDK_ROOT}/components/libraries/strerror 
  ${SDK_ROOT}/components/toolchain/cmsis/include 
  ${SDK_ROOT}/components/libraries/util 
  ${SDK_ROOT}/examples/peripheral/blinky/pca10056/blank/config
  ${SDK_ROOT}/components/libraries/balloc 
  ${SDK_ROOT}/components/libraries/ringbuf 
  ${SDK_ROOT}/modules/nrfx/hal 
  ${SDK_ROOT}/components/libraries/bsp 
  ${SDK_ROOT}/components/libraries/log 
  ${SDK_ROOT}/modules/nrfx 
  ${SDK_ROOT}/components/libraries/experimental_section_vars 
  ${SDK_ROOT}/components/libraries/delay 
  ${SDK_ROOT}/integration/nrfx 
  ${SDK_ROOT}/components/drivers_nrf/nrf_soc_nosd 
  ${SDK_ROOT}/components/libraries/atomic 
  ${SDK_ROOT}/components/boards 
  ${SDK_ROOT}/components/libraries/memobj 
  ${SDK_ROOT}/external/fprintf 
  ${SDK_ROOT}/components/libraries/log/src 
)

set(C_FLAGS
    -O3
    -g3
    -DBOARD_PCA10056
    -DBSP_DEFINES_ONLY
    -DCONFIG_GPIO_AS_PINRESET
    -DFLOAT_ABI_HARD
    -DNRF52840_XXAA
    -mcpu=cortex-m4
    -mthumb -mabi=aapcs
    -Wall -Werror
    #-mfloat-abi=hard -mfpu=fpv4-sp-d16
    -ffunction-sections -fdata-sections -fno-strict-aliasing
    -fno-builtin -fshort-enums
)


set(LD_FLAGS
    -O3
    -g3
    -mthumb -mabi=aapcs -L$(SDK_ROOT)/modules/nrfx/mdk 
    #-mcpu=cortex-m4
    #-mfloat-abi=hard -mfpu=fpv4-sp-d16
    -Wl,--gc-sections
    --specs=nano.specs
    --specs=nosys.specs
)


add_library(nrf52 ${SRCS_COMMON})

target_include_directories(nrf52 PUBLIC ${INCS_COMMON})
target_link_options(nrf52 PUBLIC ${LD_FLAGS})
#target_link_libraries(nrf52 PRIVATE ${LIBS})
target_compile_options(nrf52 PUBLIC ${C_FLAGS})
target_compile_features(nrf52 PRIVATE c_std_99)
set_target_properties(nrf52 PROPERTIES LINK_DEPENDS ${LINKER_SCRIPT})
