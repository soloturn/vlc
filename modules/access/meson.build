#
# File access modules
#

# Filesystem access module
vlc_modules += {
    'name' : 'filesystem',
    'sources' : files(['file.c', 'directory.c', 'fs.c'])
}

# Dummy access module
vlc_modules += {
    'name' : 'idummy',
    'sources' : files('idummy.c')
}

# In-memory bitstream input module
vlc_modules += {
    'name' : 'imem',
    'sources' : files('imem-access.c'),
    'dependencies' : [m_lib]
}

# Memory input module
vlc_modules += {
    'name' : 'access_imem',
    'sources' : files('imem.c')
}

# Fake sdp:// scheme input module
vlc_modules += {
    'name' : 'sdp',
    'sources' : files('sdp.c')
}

# Timecode sub-picture generator module
vlc_modules += {
    'name' : 'timecode',
    'sources' : files('timecode.c')
}

# VDR recordings access module
vlc_modules += {
    'name' : 'vdr',
    'sources' : files('vdr.c')
}


#
# Audio capture access modules
#

# ALSA input module
alsa_dep = dependency('alsa', version : '>= 1.0.24', required : get_option('alsa'))
if alsa_dep.found()
    vlc_modules += {
        'name' : 'access_alsa',
        'sources' : files('alsa.c'),
        'dependencies' : [alsa_dep]
    }
endif

# JACK input module
jack_dep = dependency('jack', version : '>= 1.9.7', required : false)

# Try jack1 instead
if not jack_dep.found()
    jack_dep = dependency('jack', version : ['>= 0.120.1', '< 1.0'],
        required : false)
endif

if jack_dep.found()
    vlc_modules += {
        'name' : 'access_jack',
        'sources' : files('jack.c'),
        'dependencies' : [jack_dep]
    }
endif

# Pulseaudio module
if pulse_dep.found()
    vlc_modules += {
        'name' : 'pulsesrc',
        'sources' : files('pulse.c'),
        'dependencies' : [pulse_dep],
        'link_with' : [libvlc_pulse],
    }
endif
