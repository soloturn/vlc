#
# Demux modules
#

# Common Xiph metadata library
xiph_meta_lib = static_library('xiph_metadata',
    sources : files('xiph_metadata.c'),
    include_directories : [vlc_include_dirs],
    install : false,
    pic : true
)

# FLAC demux
vlc_modules += {
    'name' : 'flacsys',
    'sources' : files('flac.c'),
    'include_directories' : include_directories('../packetizer'),
    'link_with' : [xiph_meta_lib]
}

# OGG demux
ogg_dep = dependency('ogg', version : '>= 1.0', required : get_option('ogg'))
if ogg_dep.found()
    vlc_modules += {
        'name' : 'ogg',
        'sources' : files('ogg.c', 'oggseek.c', 'ogg_granule.c'),
        'link_with' : [xiph_meta_lib],
        'dependencies' : [ogg_dep]
    }
endif

# Demux dump
vlc_modules += {
    'name' : 'demuxdump',
    'sources' : files('demuxdump.c')
}

# Raw DV demux
vlc_modules += {
    'name' : 'rawdv',
    'sources' : files('rawdv.c')
}

# Raw Vid demux
vlc_modules += {
    'name' : 'rawvid',
    'sources' : files('rawvid.c')
}

# au demux
vlc_modules += {
    'name' : 'au',
    'sources' : files('au.c')
}

# Raw AUD demux
vlc_modules += {
    'name' : 'rawaud',
    'sources' : files('rawaud.c')
}

# WAV demux module
vlc_modules += {
    'name' : 'wav',
    'sources' : files(['wav.c']),
}

# NSV demux
vlc_modules += {
    'name' : 'nsv',
    'sources' : files('nsv.c')
}

# MPEG PS demux
vlc_modules += {
    'name' : 'ps',
    'sources' : files('mpeg/ps.c')
}

# TODO libmodplug

# PVA demux
vlc_modules += {
    'name' : 'pva',
    'sources' : files('pva.c')
}

# AIFF demux
vlc_modules += {
    'name' : 'aiff',
    'sources' : files('aiff.c')
}

# MJPEG demux
vlc_modules += {
    'name' : 'mjpeg',
    'sources' : files('mjpeg.c')
}

# Subtitle demux
vlc_modules += {
    'name' : 'subtitle',
    'sources' : files(['subtitle.c']),
}

# ty demux
vlc_modules += {
    'name' : 'ty',
    'sources' : files('ty.c')
}

# vobsub demux
vlc_modules += {
    'name' : 'vobsub',
    'sources' : files('vobsub.c')
}

# voc demux
vlc_modules += {
    'name' : 'voc',
    'sources' : files('voc.c')
}

# xa demux
vlc_modules += {
    'name' : 'xa',
    'sources' : files('xa.c')
}

# nuv demux
vlc_modules += {
    'name' : 'nuv',
    'sources' : files('nuv.c')
}

# nsc demux
vlc_modules += {
    'name' : 'nsc',
    'sources' : files('nsc.c')
}

# TODO mpc demux plugin

# tta demux
vlc_modules += {
    'name' : 'tta',
    'sources' : files('tta.c')
}

# VC1 demux
vlc_modules += {
    'name' : 'vc1',
    'sources' : files('vc1.c')
}

# CDG demux
vlc_modules += {
    'name' : 'demux_cdg',
    'sources' : files('cdg.c')
}

# SMF demux
vlc_modules += {
    'name' : 'smf',
    'sources' : files('smf.c')
}

# TODO GME demux

# TODO SID demux

# Image demux
vlc_modules += {
    'name' : 'image',
    'sources' : files('image.c')
}

# STL demux
vlc_modules += {
    'name' : 'demux_stl',
    'sources' : files('stl.c')
}

# ASF demux
vlc_modules += {
    'name' : 'asf',
    'sources' : files('asf/asf.c', 'asf/libasf.c', 'asf/asfpacket.c')
}

# AVI demux
vlc_modules += {
    'name' : 'avi',
    'sources' : files('avi/avi.c', 'avi/libavi.c')
}

# CAF demux
vlc_modules += {
    'name' : 'caf',
    'sources' : files('caf.c'),
    'dependencies' : [m_lib]
}

# TODO avformat demux

# Directory demuxer
vlc_modules += {
    'name' : 'directory_demux',
    'sources' : files('directory.c')
}

# ES demux
vlc_modules += {
    'name' : 'es',
    'sources' : files('mpeg/es.c', '../packetizer/dts_header.c'),
    'include_directories' : include_directories('.')
}

# h.26x demux
vlc_modules += {
    'name' : 'h26x',
    'sources' : files('mpeg/h26x.c', '../packetizer/h264_nal.c'),
    'include_directories' : include_directories('.')
}

# TODO MKV demux

# MP4 demux
vlc_modules += {
    'name' : 'mp4',
    'sources' : files(
        'mp4/mp4.c',
        'mp4/fragments.c',
        'mp4/libmp4.c',
        'mp4/heif.c',
        'mp4/essetup.c',
        'mp4/meta.c',
        'asf/asfpacket.c',
    ),
    'include_directories' : include_directories('.'),
    'dependencies' : [m_lib, z_lib]
}


# mpgv demux
vlc_modules += {
    'name' : 'mpgv',
    'sources' : files('mpeg/mpgv.c')
}

# Playlist demux
vlc_modules += {
    'name' : 'playlist',
    'sources' : files(
        'playlist/asx.c',
        'playlist/b4s.c',
        'playlist/bdmv.c',
        'playlist/dvb.c',
        'playlist/ifo.c',
        'playlist/itml.c',
        'playlist/m3u.c',
        'playlist/pls.c',
        'playlist/podcast.c',
        'playlist/qtl.c',
        'playlist/ram.c',
        'playlist/sgimb.c',
        'playlist/wpl.c',
        'playlist/xspf.c',
        'playlist/playlist.c')
}

# TODO TS demux

# TODO adaptive demux

# noseek demux
vlc_modules += {
    'name' : 'noseek',
    'sources' : files('filter/noseek.c')
}
