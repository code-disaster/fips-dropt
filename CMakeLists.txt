#
# project: fips-dropt
#

fips_begin_lib(dropt)
    fips_dir(dropt GROUP .)
    fips_files(
        src/dropt.c include/dropt.h
        src/dropt_handlers.c
        src/dropt_string.c include/dropt_string.h
    )
fips_end_lib()
