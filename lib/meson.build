libvlc_sources = [
    'libvlc_internal.h',
    'media_internal.h',
    'media_list_internal.h',
    'media_player_internal.h',
    'renderer_discoverer_internal.h',
    'core.c',
    'dialog.c',
    'renderer_discoverer.c',
    'error.c',
    'log.c',
    'playlist.c',
    'picture.c',
    'video.c',
    'audio.c',
    'event.c',
    'media.c',
    'media_player.c',
    'media_list.c',
    'media_list_path.h',
    'media_list_player.c',
    'media_library.c',
    'media_discoverer.c',
]

configure_file(
    input: '../include/vlc/libvlc_version.h.in',
    output: 'libvlc_version.h',
    configuration: cdata
)

libvlc = library('vlc',
    libvlc_sources, rev_target,
    include_directories : [vlc_include_dirs],
    link_with : [vlc_libcompat, libvlccore],
    dependencies : [m_lib, threads_dep],
)
