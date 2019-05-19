# Helper libraries for modules

# Pulse audio
pulse_dep = dependency('libpulse', version : '>= 1.0', required : get_option('pulse'))

if pulse_dep.found()
    libvlc_pulse = library('vlc_pulse',
        files('audio_output/vlcpulse.c'),
        include_directories: [include_directories('audio_output'), vlc_include_dirs],
        dependencies: [pulse_dep],
        link_with: [vlc_libcompat, libvlccore],
    )
endif

# Array that holds all enabled VLC module dicts
vlc_modules = []

# access modules
subdir('access')

# audio filter modules
subdir('audio_filter')

# audio output modules
subdir('audio_output')

# codec modules
subdir('codec')

# control modules
subdir('control')

# demux modules
subdir('demux')

# gui modules
subdir('gui')

# logger modules
subdir('logger')

# misc modules
subdir('misc')

# packetizer modules
subdir('packetizer')

# service discovery modules
subdir('services_discovery')

# video chroma modules
subdir('video_chroma')

# video filter modules
subdir('video_filter')

# video output modules
subdir('video_output')

# visualization modules
subdir('visualization')

#
# Common module handling code
#
foreach module : vlc_modules
    common_args = [
        '-DMODULE_STRING="@0@"'.format(module['name']),
        '-D__PLUGIN__'
    ]

    library(module['name'] + '_plugin',
        module['sources'],
        link_with: [module.get('link_with', []), vlc_libcompat, libvlccore],
        link_args: module.get('link_args', []),
        link_depends: module.get('link_depends', []),
        include_directories: [module.get('include_directories', []), vlc_include_dirs],
        dependencies: module.get('dependencies', []),
        c_args: [module.get('c_args', []), common_args],
        cpp_args: [module.get('cpp_args', []), common_args],
        objc_args: [module.get('objc_args', []), common_args],
        install: true,
        build_by_default: true,
    )
endforeach
