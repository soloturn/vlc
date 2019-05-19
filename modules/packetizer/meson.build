#
# mpeg audio packetizer
#
vlc_modules += {
    'name' : 'packetizer_mpegaudio',
    'sources' : files('mpegaudio.c')
}

#
# h264 packetizer
#
vlc_modules += {
    'name' : 'packetizer_h264',
    'sources' : files(
        'h264_nal.c',
        'h264_slice.c',
        'h264.c',
        'hxxx_sei.c',
        'hxxx_common.c'
    )
}
