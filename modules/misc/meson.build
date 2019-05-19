# libxml2 module
libxml2_dep = dependency('libxml-2.0', version : '>= 2.5', required : get_option('libxml2'))
if libxml2_dep.found()
    vlc_modules += {
        'name' : 'xml',
        'sources' : files('xml/libxml.c'),
        'dependencies' : [libxml2_dep]
    }
endif

# medialibrary module
medialibrary_dep = dependency('medialibrary', required : get_option('medialibrary'), method : 'pkg-config')
if medialibrary_dep.found()
    vlc_modules += {
        'name' : 'medialibrary',
        'sources' : files(
            'medialibrary/medialib.cpp',
            'medialibrary/MetadataExtractor.cpp',
            'medialibrary/entities.cpp'),
        'dependencies' : [medialibrary_dep]
    }
endif
