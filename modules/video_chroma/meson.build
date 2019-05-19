# chroma copy helper library
chroma_copy_lib = static_library(
    'chroma_copy',
    files('copy.c'),
    include_directories : [vlc_include_dirs],
    pic : true
)

# cvpx chroma converter
foundation_dep = dependency('Foundation', required : false)
coremedia_dep = dependency('CoreMedia', required : false)
corevideo_dep = dependency('CoreVideo', required : false)
videotoolbox_dep = dependency('VideoToolbox', required : false)
if videotoolbox_dep.found()
    vlc_modules += {
        'name' : 'cvpx',
        'sources' : files(
            '../codec/vt_utils.c',
            'cvpx.c',
        ),
        'link_with' : [chroma_copy_lib],
        'dependencies' : [videotoolbox_dep, foundation_dep, coremedia_dep, corevideo_dep]
    }
endif
