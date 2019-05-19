#
# Audio output modules
#

# Dummy audio output
vlc_modules += {
    'name' : 'adummy',
    'sources' : files('adummy.c')
}

# File audio output
vlc_modules += {
    'name' : 'afile',
    'sources' : files('file.c')
}

# Memory audio output
vlc_modules += {
    'name' : 'amem',
    'sources' : files('amem.c')
}


# AUHAL output module (macOS only)
if have_osx
    auhal_deps = []
    auhal_deps += dependency('CoreFoundation', required: true)
    auhal_deps += dependency('CoreAudio', required: true)
    auhal_deps += dependency('AudioUnit', required: true)
    auhal_deps += dependency('AudioToolbox', required: true)

    vlc_modules += {
        'name' : 'auhal',
        'sources' : files(['auhal.c', 'coreaudio_common.c']),
        'dependencies' : [m_lib, auhal_deps]
    }
endif

# Pulseaudio out module
if pulse_dep.found()
    vlc_modules += {
        'name' : 'pulse',
        'sources' : files('pulse.c'),
        'dependencies' : [m_lib, pulse_dep],
        'link_with' : [libvlc_pulse],
    }
endif

# ALSA output module
alsa_dep = dependency('alsa', version : '>= 1.0.24', required : get_option('alsa'))
if alsa_dep.found()
    vlc_modules += {
        'name' : 'alsa',
        'sources' : files('alsa.c'),
        'dependencies' : [m_lib, alsa_dep]
    }
endif

# JACK output module
jack_dep = dependency('jack', version : '>= 1.9.7', required : false)

if not jack_dep.found()
    # Try jack1 instead
    jack_dep = dependency('jack', version : ['>= 0.120.1', '< 1.0'],
        required : false)
endif

if jack_dep.found()
    vlc_modules += {
        'name' : 'jack',
        'sources' : files('jack.c'),
        'dependencies' : [jack_dep]
    }
endif
