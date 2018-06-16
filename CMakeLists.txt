#
# project: fips-dropt
#
option(DROPT_BUILD_CPP_WRAPPER "Build C++ wrapper" OFF)

if (NOT FIPS_IMPORT)
    cmake_minimum_required(VERSION 2.8)
    get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
    include("${FIPS_ROOT_DIR}/cmake/fips.cmake")
    fips_setup(PROJECT fips-dropt)
endif()

fips_begin_lib(dropt)
    include_directories(dropt/include)
    fips_dir(dropt GROUP .)
    fips_files(
        src/dropt.c
        src/dropt_handlers.c
        src/dropt_string.c
        include/dropt.h
        include/dropt_string.h
    )
    if (DROPT_BUILD_CPP_WRAPPER)
        fips_files(
            src/droptxx.cpp
            include/droptxx.hpp
        )
    endif()
    if (FIPS_CLANG OR FIPS_GCC)
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-missing-field-initializers")
    endif()
fips_end_lib()

if (NOT FIPS_IMPORT)
    fips_begin_app(test_dropt cmdline)
        fips_dir(dropt GROUP .)
        fips_files(
            src/test_dropt.c
        )
        fips_deps(dropt)
        fips_vs_disable_warnings(4477)
    fips_end_app()

    fips_begin_app(dropt_example cmdline)
        fips_dir(dropt GROUP .)
        fips_files(
            dropt_example.c
        )
    fips_deps(dropt)
    fips_end_app()

    if (DROPT_BUILD_CPP_WRAPPER)
        fips_begin_app(droptxx_example cmdline)
            fips_dir(dropt GROUP .)
            fips_files(
                droptxx_example.cpp
            )
            fips_deps(dropt)
        fips_end_app()
    endif()

    fips_finish()
endif()
