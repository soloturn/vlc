# deinterlace common helper lib
deinterlacecommon_lib = static_library(
    'deinterlace_common',
    files('deinterlace/common.c'),
    include_directories : [vlc_include_dirs],
    pic : true
)

# deinterlace filter
vlc_modules += {
    'name' : 'deinterlace',
    'sources' : files(
        'deinterlace/deinterlace.c',
        'deinterlace/merge.c',
        'deinterlace/helpers.c',
        'deinterlace/algo_basic.c',
        'deinterlace/algo_x.c',
        'deinterlace/algo_yadif.c',
        'deinterlace/algo_phosphor.c',
        'deinterlace/algo_ivtc.c',
    ),
    # Inline ASM doesn't build with -O0
    'c_args' : ['-O2'],
    'link_with' : [deinterlacecommon_lib]
}
