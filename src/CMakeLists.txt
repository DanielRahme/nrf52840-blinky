### CMakeLists.txt for source targets

set(TARGETNAME blinky)

add_executable(${TARGETNAME} blinky.c)
target_compile_features(${TARGETNAME} PRIVATE c_std_99)
target_link_libraries(${TARGETNAME} PRIVATE nrf52)
target_link_options(${TARGETNAME} PRIVATE -Wl,--gc-sections)
target_compile_options(${TARGETNAME} PRIVATE 
    -O3
    -Wall
    -Wextra
    -nostdlib
)

