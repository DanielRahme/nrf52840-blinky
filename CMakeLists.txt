
cmake_minimum_required(VERSION 3.20)

project(
    nrf52840-dk
    VERSION 0.1
    DESCRIPTION "Example project for nrf52840-dk board."
    LANGUAGES C CXX ASM
)

include(ExternalProject)

### Download and unpack nrfjprog programmer
set(NRFJPROGPROJ_SRC_DIR ${PROJECT_BINARY_DIR}/nrfjprogproj)
set(NRFJPROG_TARBALL nrf-command-line-tools-10.15.0_Linux-amd64.tar.gz)

ExternalProject_Add(nrfjprogproj
    URL https://www.nordicsemi.com/-/media/Software-and-other-downloads/Desktop-software/nRF-command-line-tools/sw/Versions-10-x-x/10-15-0/nrf-command-line-tools-10.15.0_amd.zip
    URL_HASH SHA256=E30B1C94508AFE70EA3AD5EEFB6B9914EE70B9DCE17F14412BCBBB53D1AAD9BD

    SOURCE_DIR ${NRFJPROGPROJ_SRC_DIR}
    CONFIGURE_COMMAND ${CMAKE_COMMAND} -E tar xzvf ${NRFJPROGPROJ_SRC_DIR}/${NRFJPROG_TARBALL}
    BUILD_COMMAND ${CMAKE_COMMAND} -E echo "Skipping build step."
    INSTALL_COMMAND cmake -E echo "Skipping install step."
)
ExternalProject_Get_Property(nrfjprogproj BINARY_DIR)

### Make the nrjfprog a target executable
set(NRFJPROG_BIN_DIR ${BINARY_DIR})
add_executable(nrfjprog IMPORTED)
set_target_properties(nrfjprog
    PROPERTIES
    IMPORTED_LOCATION "${NRFJPROG_BIN_DIR}/nrf-commad-line-tools/bin/nrfjprog" 
)

add_dependencies(nrfjprog nrfjprogproj)



### Nordic Semi SDK
set(SDK_SRC_DIR ${PROJECT_SOURCE_DIR}/external/nrf5-sdk)

ExternalProject_Add(nordic-sdk-proj
    URL https://www.nordicsemi.com/-/media/Software-and-other-downloads/SDKs/nRF5/Binaries/nRF5_SDK_17.1.0_ddde560.zip
    #URL_HASH SHA256=E30B1C94508AFE70EA3AD5EEFB6B9914EE70B9DCE17F14412BCBBB53D1AAD9BD

    CONFIGURE_COMMAND ""
    SOURCE_DIR ${SDK_SRC_DIR}
    #BUILD_COMMAND ${CMAKE_COMMAND} -E tar xzvf ${NRFJPROGPROJ_SRC_DIR}/${NRFJPROG_TARBALL}
    #BUILD_COMMAND ${MAKE}
    #CONFIGURE_COMMAND ${MAKE}
    BUILD_COMMAND ${CMAKE_COMMAND} -E echo "Skipping build step."
    INSTALL_COMMAND cmake -E echo "Skipping install step."
)
#ExternalProject_Get_Property(nrfjprogproj BINARY_DIR)

add_subdirectory(external)
add_subdirectory(src)
