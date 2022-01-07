cmake_minimum_required(VERSION 3.18)
project(DxTest)

if (NOT CMAKE_HOST_WIN32)
    message("Using DXVK-native for D3D9 API")

    include(ExternalProject)
    ExternalProject_Add(dxvk-native
            GIT_REPOSITORY    https://github.com/Joshua-Ashton/dxvk-native
            GIT_TAG           origin/master
            GIT_SHALLOW       ON
            BUILD_ALWAYS      OFF
            CONFIGURE_HANDLED_BY_BUILD ON
            CONFIGURE_COMMAND CC=gcc-10 && CXX=g++-10 && meson ../dxvk-native
            BUILD_COMMAND     ninja
            INSTALL_COMMAND   ""
    )
    ExternalProject_Get_property(dxvk-native SOURCE_DIR BINARY_DIR)
    message("inc dir ${SOURCE_DIR}/include/native/directx")
    set(DXVK_NATIVE_INCLUDE_DIRS
            "${SOURCE_DIR}/include/native/directx"
            "${SOURCE_DIR}/include/native/windows"
    )
    set(DXVK_NATIVE_D3D9_LIB ${BINARY_DIR}/src/d3d9/libdxvk_d3d9.so)

    ADD_CUSTOM_TARGET(dependencies ALL DEPENDS dxvk-native)

    include_directories("${DXVK_NATIVE_INCLUDE_DIRS}")
endif()

add_subdirectory(src)
