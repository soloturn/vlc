#
# Encoder and decoder modules
#

# Dummy codec
vlc_modules += {
    'name' : 'ddummy',
    'sources' : files('ddummy.c')
}

# Dummy codec
vlc_modules += {
    'name' : 'edummy',
    'sources' : files('edummy.c')
}

# SPDIF codec
vlc_modules += {
    'name' : 'spdif',
    'sources' : files('spdif.c')
}

# a52 codec
a52_lib = cc.find_library('a52', required: get_option('a52'))
if a52_lib.found()
    vlc_modules += {
        'name' : 'a52',
        'sources' : files('a52.c'),
        'dependencies' : [a52_lib]
    }
endif

# DTS Coherent Acoustics decoder
dca_dep = dependency('libdca', version : '>= 0.0.5', required : get_option('dca'))
if dca_dep.found()
    vlc_modules += {
        'name' : 'dca',
        'sources' : files('dca.c'),
        'dependencies' : [dca_dep, m_lib]
    }
endif

# adpcm codec
vlc_modules += {
    'name' : 'adpcm',
    'sources' : files('adpcm.c')
}

# AES3 codec
vlc_modules += {
    'name' : 'aes3',
    'sources' : files('aes3.c')
}

# faad decoder plugin
faad_lib = cc.find_library('faad', required: get_option('faad'))
if faad_lib.found()
    vlc_modules += {
        'name' : 'faad',
        'sources' : files('faad.c'),
        'dependencies' : [faad_lib, m_lib]
    }
endif

# g711 codec
vlc_modules += {
    'name' : 'g711',
    'sources' : files('g711.c')
}

# libfluidsynth (MIDI synthetizer) plugin
fluidsynth_option = get_option('fluidsynth')

if not fluidsynth_option.disabled()
    fluidsynth_dep = dependency('fluidsynth', version : '>= 1.1.2', required : false)
    if not fluidsynth_dep.found()
        fluidsynth_dep = dependency('fluidlite', required : false)
    endif

    if fluidsynth_option.enabled() and not fluidsynth_dep.found()
        error('Neither fluidsynth or fluidlite were found.')
    endif
else
    fluidsynth_dep = dependency('', required : false)
endif

if fluidsynth_dep.found()
    vlc_modules += {
        'name' : 'fluidsynth',
        'sources' : files('fluidsynth.c'),
        'dependencies' : [fluidsynth_dep, m_lib]
    }
endif

# Audiotoolbox MIDI plugin (Darwin only)
if host_system == 'darwin'
    corefoundation_dep = dependency('CoreFoundation', required : true)
    audiounit_dep = dependency('AudioUnit', required : true)
    audiotoolbox_dep = dependency('AudioToolbox', required : true)

    vlc_modules += {
        'name' : 'audiotoolboxmidi',
        'sources' : files('audiotoolbox_midi.c'),
        'dependencies' : [corefoundation_dep, audiounit_dep, audiotoolbox_dep]
    }
endif

# LPCM codec
vlc_modules += {
    'name' : 'lpcm',
    'sources' : files('lpcm.c')
}

# TODO libmad codec

# Ulead DV Audio codec
vlc_modules += {
    'name' : 'uleaddvaudio',
    'sources' : files('uleaddvaudio.c')
}

# WMA fixed codec
vlc_modules += {
    'name' : 'wma_fixed',
    'sources' : files(
        'wmafixed/fft.c',
        'wmafixed/mdct.c',
        'wmafixed/wma.c',
        'wmafixed/wmadeci.c',
        'wmafixed/bitstream.c',
        'wmafixed/wmafixed.c')
}

# Raw PCM demux module
vlc_modules += {
    'name' : 'araw',
    'sources' : files(['araw.c']),
}

# libmpg123 decoder
mpg123_dep = dependency('libmpg123', required : get_option('mpg123'))
if mpg123_dep.found()
    vlc_modules += {
        'name' : 'mpg123',
        'sources' : files('mpg123.c'),
        'dependencies' : [mpg123_dep]
    }
endif

#
# Video codecs
#

# CDG codec
vlc_modules += {
    'name' : 'cdg',
    'sources' : files('cdg.c')
}

# libmpeg2 decoder
mpeg2_dep = dependency('libmpeg2', version : '> 0.3.2', required : get_option('mpeg2'))
if mpeg2_dep.found()
    vlc_modules += {
        'name' : 'libmpeg2',
        'sources' : files('libmpeg2.c', 'synchro.c'),
        'dependencies' : [mpeg2_dep]
    }
endif

# Raw video codec
vlc_modules += {
    'name' : 'rawvideo',
    'sources' : files('rawvideo.c')
}

# RTP video codec
vlc_modules += {
    'name' : 'rtpvideo',
    'sources' : files('rtpvideo.c')
}

# Dirac decoder and encoder using schroedinger
schroedinger_dep = dependency('schroedinger-1.0', version : '>= 1.0.10', required : get_option('schroedinger'))
if schroedinger_dep.found()
    vlc_modules += {
        'name' : 'schroedinger',
        'sources' : files('schroedinger.c'),
        'dependencies' : [schroedinger_dep]
    }
endif

#
# Image codecs
#

# TODO PNG codec

# TODO JPEG codec

# TODO BPG codec

# SVG image decoder
rsvg_dep = dependency('librsvg-2.0', version : '>= 2.9.0', required : get_option('rsvg'))
cairo_dep = dependency('cairo', version : '>= 1.13.1', required : get_option('cairo'))
if rsvg_dep.found() and cairo_dep.found()
    vlc_modules += {
        'name' : 'svgdec',
        'sources' : files('svg.c'),
        'dependencies' : [rsvg_dep, cairo_dep]
    }
endif

# TODO SDL image codec

# TODO xwd


#
# SPU codecs
#

# TODO Substation Alpha subtitle decoder (libass)

# Closed captioning decoder
vlc_modules += {
    'name' : 'cc',
    'sources' : files('cc.c', 'cea708.c')
}

# cvdsub decoder
vlc_modules += {
    'name' : 'cvdsub',
    'sources' : files('cvdsub.c')
}

# dvbsub decoder
vlc_modules += {
    'name' : 'dvbsub',
    'sources' : files('dvbsub.c')
}

# TODO aribsub

# scte18 decoder
vlc_modules += {
    'name' : 'scte18',
    'sources' : files('scte18.c', 'atsc_a65.c')
}

# scte27 decoder
vlc_modules += {
    'name' : 'scte27',
    'sources' : files('scte27.c')
}

# SPU decoder
vlc_modules += {
    'name' : 'spudec',
    'sources' : files('spudec/spudec.c', 'spudec/parse.c')
}

# STL codec
vlc_modules += {
    'name' : 'stl',
    'sources' : files('stl.c')
}

# subsdec
vlc_modules += {
    'name' : 'subsdec',
    'sources' : files('subsdec.c')
}

# subsusf
vlc_modules += {
    'name' : 'subsusf',
    'sources' : files('subsusf.c')
}

# TTML decoder
vlc_modules += {
    'name' : 'ttml',
    'sources' : files('ttml/substtml.c', 'ttml/ttml.c', '../demux/ttml.c'),
    'include_directories' : include_directories('.')
}

# TODO WebVTT

# svcdsub
vlc_modules += {
    'name' : 'svcdsub',
    'sources' : files('svcdsub.c')
}

# t140
vlc_modules += {
    'name' : 't140',
    'sources' : files('t140.c')
}

# telx
vlc_modules += {
    'name' : 'telx',
    'sources' : files('telx.c')
}

# textst
vlc_modules += {
    'name' : 'textst',
    'sources' : files('textst.c')
}

# TODO zvbi

# substx3g
vlc_modules += {
    'name' : 'substx3g',
    'sources' : files('substx3g.c')
}

#
# Xiph.org codecs
#

ogg_dep = dependency('ogg', required : get_option('ogg'))

# FLAC codec
flac_dep = dependency('flac', required : get_option('flac'))
if flac_dep.found()
    vlc_modules += {
        'name' : 'flac',
        'sources' : files('flac.c'),
        'dependencies' : [flac_dep]
    }
endif

# TODO Kate codec
#kate_dep = dependency('kate', version : '>= 0.3.0', required : false)
#if kate_dep.found()
#    vlc_modules += {
#        'name' : 'kate',
#        'sources' : files('kate.c')
#    }
#endif

# Opus codec
opus_dep = dependency('opus', version : '>= 1.0.3', required : get_option('opus'))
if opus_dep.found() and ogg_dep.found()
    vlc_modules += {
        'name' : 'opus',
        'sources' : files('opus.c', 'opus_header.c'),
        'dependencies' : [opus_dep, ogg_dep]
    }
endif

# TODO speex

# Theora codec
theoraenc_dep = dependency('theoraenc', required : get_option('theoraenc'))
theoradec_dep = dependency('theoradec', version : '>= 1.0', required : get_option('theoradec'))
if theoraenc_dep.found() and theoradec_dep.found() and ogg_dep.found()
    vlc_modules += {
        'name' : 'theora',
        'sources' : files('theora.c'),
        'dependencies' : [theoraenc_dep, theoradec_dep, ogg_dep]
    }
endif

# Daala decoder plugin
daaladec_dep = dependency('daaladec', required : get_option('daaladec'))
daalaenc_dep = dependency('daalaenc', required : get_option('daalaenc'))
if daaladec_dep.found() and daalaenc_dep.found()
    vlc_modules += {
        'name' : 'daala',
        'sources' : files('daala.c'),
        'dependencies' : [daaladec_dep, daalaenc_dep]
    }
endif

# TODO tremor

# TODO vorbis

# OggSpots decoder
if ogg_dep.found()
    vlc_modules += {
        'name' : 'oggspots',
        'sources' : files('oggspots.c')
    }
endif

# videotoolbox
foundation_dep = dependency('Foundation', required : false)
coremedia_dep = dependency('CoreMedia', required : false)
corevideo_dep = dependency('CoreVideo', required : false)
videotoolbox_dep = dependency('VideoToolbox', required : false)
if videotoolbox_dep.found()
    vlc_modules += {
        'name' : 'videotoolbox',
        'sources' : files(
            'vt_utils.c',
            'videotoolbox.m',
            'hxxx_helper.c',
            '../packetizer/hxxx_nal.c',
            '../packetizer/hxxx_sei.c',
            '../packetizer/h264_slice.c',
            '../packetizer/h264_nal.c',
            '../packetizer/hevc_nal.c'
        ),
        'dependencies' : [videotoolbox_dep, foundation_dep, coremedia_dep, corevideo_dep],
        'objc_args' : ['-fobjc-arc']
    }
endif

# TODO FFmpeg/libav

# TODO avcodec hardware acceleration

# TODO omxil


#
# x26x encoders
#

# x265 encoder
x265_dep = dependency('x265', required : get_option('x265'))
if x265_dep.found()
    vlc_modules += {
        'name' : 'x265',
        'sources' : files('x265.c'),
        'dependencies' : [x265_dep]
    }
endif

# TODO x262

# TODO x264

# TODO x26410b


#
# Misc codecs
#

# fdkaac encoder
fdkaac_dep = dependency('fdk-aac', required : get_option('fdk-aac'))
if fdkaac_dep.found()
    vlc_modules += {
        'name' : 'fdkaac',
        'sources' : files('fdkaac.c'),
        'dependencies' : [fdkaac_dep]
    }
endif

# Shine MPEG Audio Layer 3 encoder
shine_dep = dependency('shine', version : '>= 3.0.0', required : get_option('shine'))
if shine_dep.found()
    vlc_modules += {
        'name' : 'shine',
        'sources' : files('shine.c'),
        'dependencies' : [shine_dep]
    }
endif

# TODO libvpx

# Experimental AV1 codec
aom_dep = dependency('aom', required : get_option('aom'))
if aom_dep.found()
    vlc_modules += {
        'name' : 'aom',
        'sources' : files('aom.c'),
        'dependencies' : [aom_dep]
    }
endif

# Twolame MPEG Audio Layer 2 encoder
twolame_dep = dependency('twolame', required : get_option('twolame'))
if twolame_dep.found()
    vlc_modules += {
        'name' : 'twolame',
        'sources' : files('twolame.c'),
        'dependencies' : [twolame_dep],
        'c_args' : ['-DLIBTWOLAME_STATIC']
    }
endif


#
# Hardware encoders
#

# TODO crystalhd

# QSV encoder
qsv_dep = dependency('libmfx', required : get_option('mfx'))
if qsv_dep.found()
    vlc_modules += {
        'name' : 'qsv',
        'sources' : files('qsv.c'),
        'dependencies' : [qsv_dep]
    }
endif


#
# External frameworks
#

# TODO dmo

# TODO mft

# TODO gstdecode
