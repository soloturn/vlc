# Zeroconf services discovery
avahi_dep = dependency('avahi-client', version : '>= 0.6', required : get_option('avahi'))
if avahi_dep.found()
    vlc_modules += {
        'name' : 'avahi',
        'sources' : files('avahi.c'),
        'dependencies' : [avahi_dep]
    }
endif

# MTP devices support
mtp_dep = dependency('libmtp', version : '>= 1.0.0', required : get_option('mtp'))
if mtp_dep.found()
    vlc_modules += {
        'name' : 'mtp',
        'sources' : files('mtp.c'),
        'dependencies' : [mtp_dep]
    }
endif

# UPnP Plugin (Intel SDK)
upnp_dep = dependency('libupnp', required : get_option('upnp'))
if upnp_dep.found()
    vlc_modules += {
        'name' : 'upnp',
        'sources' : files('upnp.cpp', 'upnp-wrapper.cpp'),
        'dependencies' : [upnp_dep]
    }
endif

# mDNS using libmicrodns
microdns_dep = dependency('microdns', required : get_option('microdns'))
if microdns_dep.found()
    vlc_modules += {
        'name' : 'microdns',
        'sources' : files('microdns.c'),
        'dependencies' : [microdns_dep, socket_libs]
    }
endif
