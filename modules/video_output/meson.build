#
# Video output modules
#

# Dummy video output
vlc_modules += {
    'name' : 'vdummy',
    'sources' : files('vdummy.c')
}

# Dummy window provider
vlc_modules += {
    'name' : 'wdummy',
    'sources' : files('wdummy.c')
}

# YUV video output
vlc_modules += {
    'name' : 'yuv',
    'sources' : files('yuv.c')
}

# Flaschen video output
vlc_modules += {
    'name' : 'flaschen',
    'sources' : files('flaschen.c')
}

# Coloured ASCII art (Caca)
# TODO: Solve meson bug https://github.com/mesonbuild/meson/issues/3849
#caca_dep = dependency('caca', version : '>= 0.99.beta14', required : get_option('caca'))
caca_dep = dependency('', required : get_option('caca'))
if caca_dep.found()
    vlc_modules += {
        'name' : 'caca',
        'sources' : files('caca.c'),
        'dependencies' : [caca_dep]
    }
    # TODO: https://github.com/mesonbuild/meson/issues/3849
    # TODO: this needs for X11(?) some equivalent of:
    # $(X_LIBS) $(X_PRE_LIBS) -lX11
endif

# Kernel Mode Setting
drm_dep = dependency('libdrm', version : '>= 2.4.83', required : get_option('drm'))
if caca_dep.found()
    vlc_modules += {
        'name' : 'kms',
        'sources' : files('kms.c'),
        'dependencies' : [drm_dep]
    }
endif

# Linux FrameBuffer
if cc.check_header('linux/fb.h')
    vlc_modules += {
        'name' : 'fb',
        'sources' : files('fb.c')
    }
endif

# TODO Android EGL stuff

# TODO OS2 KVA stuff

# TODO drawable

# TODO inhibit

# TODO egl win32

# TODO wingdi

# TODO glwin32

# TODO directdraw

# TODO direct3d11

# TODO direct3d9

# TODO ALL THE OTHER STUFF!

#
# OpenGL video outputs
#

opengl_common_sources = files(
    'opengl/vout_helper.c',
    'opengl/fragment_shaders.c',
    'opengl/converter_sw.c'
    )

opengl_common_deps = []

if have_osx
    quartz_dep = dependency('QuartzCore', required: true)
    opengl_dep = dependency('appleframeworks', modules : ['OpenGL'], required : true)
    cocoa_dep = dependency('Cocoa', required: true)

    vlc_modules += {
        'name' : 'vout_macosx',
        'sources' : [files('macosx.m'), opengl_common_sources],
        'c_args' : ['-DHAVE_GL_CORE_SYMBOLS'],
        'dependencies' : [m_lib, cocoa_dep, opengl_dep, opengl_common_deps]
    }

    vlc_modules += {
        'name' : 'caopengllayer',
        'sources' : [files('caopengllayer.m'), opengl_common_sources],
        'c_args' : ['-DHAVE_GL_CORE_SYMBOLS'],
        'dependencies' : [m_lib, cocoa_dep, opengl_dep, quartz_dep, opengl_common_deps]
    }
endif
