vlc_libcompat = []

if libcompat_sources.length() > 0
    actual_libcompat = static_library(
        'compat',
        libcompat_sources,
        include_directories : [vlc_include_dirs],
        dependencies : [m_lib],
        pic : true
    )

    vlc_libcompat += actual_libcompat
endif
