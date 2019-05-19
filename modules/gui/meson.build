#
# Graphical user interface (GUI) modules
#

# Qt interface module
subdir('qt')

# ncurses text-based interface
ncurses_dep = dependency('ncursesw', required : false)
if ncurses_dep.found()
    vlc_modules += {
        'name' : 'ncurses',
        'sources' : files('ncurses.c'),
        'dependencies' : [m_lib, threads_dep, ncurses_dep]
    }
endif

# Minimal macOS interface
if have_osx
    cocoa_dep = dependency('Cocoa', required: true)
    vlc_modules += {
        'name' : 'minimal_macosx',
        'sources' : files(
            'minimal_macosx/intf.m',
            'minimal_macosx/misc.m',
            'minimal_macosx/VLCMinimalVoutWindow.m',
            'minimal_macosx/macosx.c'),
        'dependencies' : [cocoa_dep],
        'objc_args' : ['-fobjc-arc', '-fobjc-exceptions']
    }
endif
