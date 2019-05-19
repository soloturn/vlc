vlc_sources = []

vlc_deps = [m_lib, dl_lib, threads_dep]

if host_system == 'darwin'
    vlc_sources += ['darwinvlc.m']
    vlc_deps += dependency('CoreFoundation', required: true)
    vlc_deps += dependency('Cocoa', required: true)
elif host_system == 'windows'
    vlc_sources += ['winvlc.c']
else
    vlc_sources += ['vlc.c', 'override.c']
endif

executable('vlc',
    vlc_sources,
    link_with: [libvlc],
    include_directories: [vlc_include_dirs],
    dependencies: vlc_deps,
    # HACK HACK HACK HACK HACK HACK HACK HACK HACK HACK HACK HACK
    # TODO FIXME BAD BAD HACK for macOS:
    # We need to set the build rpath here becase the Qt frameworks
    # use @rpath and else would fail to load on macOS.
    # Eventually this should be solved by:
    # https://github.com/mesonbuild/meson/pull/3402
    # HACK HACK HACK HACK HACK HACK HACK HACK HACK HACK HACK HACK
    build_rpath: '/Applications/Qt/5.10.1/clang_64/lib'
)
