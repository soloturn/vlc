project('VLC', ['c', 'cpp'],
    version : '4.0.0-dev',
    default_options : ['c_std=gnu11', 'cpp_std=gnu++11'])

vlc_copyright_years = '1996-2018'
vlc_version_codename = 'Otto Chriek'

vlc_version = meson.project_version()
version_arr = vlc_version.split('.')
version_arr_last = version_arr[2].split('-')

vlc_version_major = version_arr.get(0).to_int()
vlc_version_minor = version_arr.get(1).to_int()
vlc_version_revision = version_arr_last.get(0).to_int()
vlc_version_type  = version_arr_last.get(1, '')

vlc_src_root = meson.current_source_dir()

cdata = configuration_data()
vlc_include_dirs = include_directories('.', 'include')

gen_vlc_about = find_program('extras/buildsystem/gen-vlc-about.py')
vlc_about = custom_target('vlc_about.h',
                        input:   ['COPYING', 'THANKS', 'AUTHORS'],
                        output:  ['vlc_about.h'],
                        command: [gen_vlc_about,
                                   '@INPUT0@',
                                   '@INPUT1@',
                                   '@INPUT2@',
                                   '@OUTPUT@'])

add_project_arguments('-DHAVE_CONFIG_H=1', language : ['c', 'cpp', 'objc'])

cc = meson.get_compiler('c')
cpp = meson.get_compiler('cpp')
host_system = host_machine.system()

if host_system == 'darwin'
    add_languages('objc')
    add_project_arguments('-mmacosx-version-min=10.10',
        language: ['c', 'cpp', 'objc'])
    add_project_link_arguments('-mmacosx-version-min=10.10',
        language: ['c', 'cpp', 'objc'])
endif

#
# General feature defines
#
vlc_conf_prefix = ''

# TODO FIXME: Properly check this once I figure out how
cdata.set('X_DISPLAY_MISSING', 1)

feature_defines = [
    ['_REENTRANT',                  1], # Define so that reentrant versions of several functions get declared
    ['_THREAD_SAFE',                1], # Same as _REENTANT for some other OSes
    ['_GNU_SOURCE',                 1], # Enable GNU extensions on systems that have them
    ['_POSIX_PTHREAD_SEMANTICS',    1], # Enable threading extensions on Solaris
    ['__EXTENSIONS__',              1], # Enable general extensions on Solaris
    ['_FILE_OFFSET_BITS',           64], # Define to 64 for large files support
]

if host_system == 'darwin'
    # Define to use macros for libintl prefixed functions (see libgnuintl.h)
    feature_defines += [
        ['_INTL_REDIRECT_MACROS', 1]
    ]
endif

foreach d : feature_defines
    cdata.set(d.get(0), d.get(1))
    vlc_conf_prefix = vlc_conf_prefix + '#define @0@ @1@\n'.format(d.get(0), d.get(1))
endforeach

#
# Check for libraries
#

# zlib library
z_lib = cc.find_library('z', required: false)

# Math library
m_lib = cc.find_library('m', required: false)

# Dynamic library loading library
dl_lib = cc.find_library('dl', required: false)

# Gettext
if cc.has_function('ngettext', prefix : '#include <libintl.h>')
    libintl_lib = []
    cdata.set('HAVE_GETTEXT', 1)
    cdata.set('ENABLE_NLS', 1)
else
    libintl_lib = cc.find_library('intl', required : false)
    if libintl_lib.found()
        cdata.set(ENABLE_NLS, 1)
    endif
endif

# Threads
threads_dep = dependency('threads', required: true)

#
# Check for headers
#

check_headers = [
    ['HAVE_THREADS_H',              'threads.h'],
    ['HAVE_NETINET_TCP_H',          'netinet/tcp.h'],
    ['HAVE_SEARCH_H',               'search.h'],
    ['HAVE_SYS_UIO_H',              'sys/uio.h'],
    ['HAVE_SYS_SOCKET_H',           'sys/socket.h'],
    ['HAVE_NET_IF_H',               'net/if.h'],
    ['HAVE_EXECINFO_H',             'execinfo.h'],
    ['HAVE_FEATURES_H',             'features.h'],
    ['HAVE_GETOPT_H',               'getopt.h'],
    ['HAVE_LINUX_DCCP_H',           'linux/dccp.h'],
    ['HAVE_LINUX_MAGIC_H',          'linux/magic.h'],
    ['HAVE_NETINET_UDPLITE_H',      'netinet/udplite.h'],
    ['HAVE_PTHREAD_H',              'pthread.h'],
    ['HAVE_SYS_EVENTFD_H',          'sys/eventfd.h'],
    ['HAVE_SYS_MOUNT_H',            'sys/mount.h'],
    ['HAVE_SYS_SHM_H',              'sys/shm.h'],
    ['HAVE_SYS_SOCKET_H',           'sys/socket.h'],
    ['HAVE_SYS_SOUNDCARD_H',        'sys/soundcard.h'],
    ['HAVE_SYS_UIO_H',              'sys/uio.h'],
    ['HAVE_VALGRIND_VALGRIND_H',    'valgrind/valgrind.h'],
    ['HAVE_X11_XLIB_H',             'X11/Xlib.h'],
    ['HAVE_XLOCALE_H',              'xlocale.h'],
    ['HAVE_ZLIB_H',                 'zlib.h'],
]

foreach h : check_headers
  if cc.check_header(h.get(1))
    cdata.set(h.get(0), 1)
  endif
endforeach


#
# Darwin specific checks
#

if host_system == 'darwin'

    # Check if compiling for iOS
    have_ios = cc.get_define('TARGET_OS_IPHONE',
        prefix : '#include <TargetConditionals.h>') == '1'

    # Check if compiling for tvOS
    have_tvos = cc.get_define('TARGET_OS_TV',
        prefix : '#include <TargetConditionals.h>') == '1'

    # If none of the above, assume compiling for macOS
    have_osx = not have_ios and not have_tvos

else
    have_ios = false
    have_tvos = false
    have_osx = false
endif


#
# Windows and MinGW checks
#

have_mingw = false
mingw_libs = []

if host_system == 'windows'

    # Defines needed for Windows
    windows_defines = [
        ['UNICODE',                 1], # Define to 1 for Unicode (Wide Chars) APIs
        ['_UNICODE',                1], # Define to 1 for Unicode (Wide Chars) APIs
        ['_WIN32_WINNT',            '0x0601'], # Define for Windows 7 APIs
        ['_WIN32_IE',               '0x0600'], # Define for IE 6.0 (and shell) APIs
    ]

    foreach d : windows_defines
        cdata.set(d.get(0), d.get(1))
        vlc_conf_prefix = vlc_conf_prefix + '#define @0@ @1@\n'.format(d.get(0), d.get(1))
    endforeach

    mingw_check = '''
    #ifndef __MINGW32__
    # error Not compiling with mingw
    #endif
    '''

    # Check if MinGW is used at all
    if cc.compiles(mingw_check)

        # Check which kind of MinGW
        mingw_version_major = cc.get_define('__MINGW64_VERSION_MAJOR',
            prefix : '#include <_mingw.h>')

        if mingw_version_major == ''
            error('Cannot compile with MinGW, use MinGW-w64 >= 5.0 instead.')
        endif

        # Check that MinGW w64 is at least 5.0
        if mingw_version_major.to_int() < 5
            error('MinGW-w64 5.0 or higher required!')
        endif

        have_mingw = true

        mingw_version_minor = cc.get_define('__MINGW64_VERSION_MINOR',
            prefix : '#include <_mingw.h>')

        mingw_version = '@0@.@1@'.format(mingw_version_major, mingw_version_minor)
        message('Using MinGW-w64 ' + mingw_version)

        # Defines needed for MinGW

        mingw_defines = [
            ['__USE_MINGW_ANSI_STDIO',      1], # Define to force use of MinGW printf
            ['_ISOC99_SOURCE',              1], # Extensions to ISO C89 from ISO C99
            ['_ISOC11_SOURCE',              1], # Extensions to ISO C99 from ISO C11
            ['_POSIX_SOURCE',               1], # IEEE Std 1003.1
            ['_POSIX_C_SOURCE',             '200809L'], #IEEE Std 1003.1
            ['_XOPEN_SOURCE',               700], # POSIX and XPG 7th edition
            ['_XOPEN_SOURCE_EXTENDED',      1], # XPG things and X/Open Unix extensions
            ['_BSD_SOURCE',                 1], # ISO C, POSIX, and 4.3BSD things
            ['_SVID_SOURCE',                1], # ISO C, POSIX, and SVID things
        ]

        foreach d : mingw_defines
            cdata.set(d.get(0), d.get(1))
            vlc_conf_prefix = vlc_conf_prefix + '#define @0@ @1@\n'.format(d.get(0), d.get(1))
        endforeach

        # Check for the need to link to the mingwex lib for MinGW-w64 32bit
        mingwex_lib = cc.find_library('mingwex', required : false)

        if mingwex_lib.found() and not cc.find_library('mingw32', required: false).found()
            mingw_libs += mingwex_lib
        endif

        # Check for fnative-struct or mms-bitfields support for MinGW
        if cc.has_argument('-mms-bitfields')
            add_project_arguments('-mms-bitfields',
                language : ['c', 'cpp'])
            # Check for the warning flag without "-Wno-", GCC accepts
            # -Wno-<anything> for unsupported warnings, which can trigger
            # other warnings instead.
            if cc.has_argument('-Wincompatible-ms-struct')
                add_project_arguments('-Wno-incompatible-ms-struct',
                    language : ['c', 'cpp'])
            endif
        elif cc.has_argument('-fnative-struct')
            add_project_arguments('-fnative-struct',
                    language : ['c', 'cpp'])
        endif

        # DEP, ASLR, NO SEH
        add_project_link_arguments('-Wl,--nxcompat', '-Wl,--no-seh', '-Wl,--dynamicbase',
            language: ['c', 'cpp'])
    endif

endif


#
# Check for functions
# Entry format: [TO_DEFINE_IF_TRUE, function_name, prefix]

# General functions
check_functions = [
    ['HAVE_ACCEPT4',        'accept4',          '#include <sys/socket.h>'],
    ['HAVE_DAEMON',         'daemon',           '#include <stdlib.h>'],
    ['HAVE_FCNTL',          'fcntl',            '#include <fcntl.h>'],
    ['HAVE_FLOCK',          'flock',            '#include <sys/file.h>'],
    ['HAVE_FSTATVFS',       'fstatvfs',         '#include <sys/statvfs.h>'],
    ['HAVE_FORK',           'fork',             '#include <unistd.h>'],
    ['HAVE_GETPWUID_R',     'getpwuid_r',       '#include <pwd.h>'],
    ['HAVE_ISATTY',         'isatty',           '#include <unistd.h>'],
    ['HAVE_MEMALIGN',       'memalign',         '#include <malloc.h>'],
    ['HAVE_MKOSTEMP',       'mkostemp',         '#include <unistd.h>'],
    ['HAVE_MMAP',           'mmap',             '#include <sys/mman.h>'],
    ['HAVE_OPEN_MEMSTREAM', 'open_memstream',   '#include <stdio.h>'],
    ['HAVE_OPENAT',         'openat',           '#include <fcntl.h>'],
    ['HAVE_PIPE2',          'pipe2',            '#include <unistd.h>'],
    ['HAVE_POSIX_FADVISE',  'posix_fadvise',    '#include <fcntl.h>'],
    ['HAVE_STRICMP',        'stricmp',          '#include <string.h>'],
    ['HAVE_STRCOLL',        'strcoll',          '#include <string.h>'],

    ['HAVE_USELOCALE',      'uselocale',        '#include <locale.h>'],
    ['HAVE_USELOCALE',      'uselocale',        '#include <xlocale.h>'],
    ['HAVE_NEWLOCALE',      'newlocale',        '#include <locale.h>'],
    ['HAVE_NEWLOCALE',      'newlocale',        '#include <xlocale.h>'],

    ['HAVE_GETENV',         'getenv',           '#include <stdlib.h>'],
    ['HAVE_POSIX_MEMALIGN', 'posix_memalign',   '#include <stdlib.h>'],

    ['HAVE_IF_NAMETOINDEX', 'if_nametoindex',   '#include <net/if.h>'],
    ['HAVE_IF_NAMEINDEX',   'if_nameindex',     '#include <net/if.h>'],

    ['HAVE_SIGWAIT',        'sigwait',          '#include <signal.h>'],
    ['HAVE_BACKTRACE',      'backtrace',        '#include <execinfo.h>'],
    ['HAVE__LOCK_FILE',     '_lock_file',       '#include <stdio.h>'],
]

# Linux specific functions
# recvmmsg memfd_create
check_functions_linux = [
    ['HAVE_EVENTFD',            'eventfd',              '#include <sys/eventfd.h>'],
    ['HAVE_VMSPLICE',           'vmsplice',             '#include <fcntl.h>'],
    ['HAVE_SCHED_GETAFFINITY',  'sched_getaffinity',    '#include <sched.h>'],
    ['HAVE_RECVMMSG',           'recvmmsg',             '#include <sys/socket.h>'],
    ['HAVE_MEMFD_CREATE',       'memfd_create',         '#include <sys/memfd.h>'],
]

if host_system == 'linux'
    check_functions += check_functions_linux
endif

foreach f : check_functions
    if cc.has_function(f.get(1), prefix : vlc_conf_prefix + f.get(2))
        cdata.set(f.get(0), 1)
    endif
endforeach

# Libcompat functions (if missing, provided in compat)
# Entry format: [TO_DEFINE_IF_TRUE, function_name, prefix]
libcompat_functions = [
    ['HAVE_ALIGNED_ALLOC',  'aligned_alloc',    '#include <stdlib.h>'],
    ['HAVE_ATOF',           'atof',             '#include <stdlib.h>'],
    ['HAVE_ATOLL',          'atoll',            '#include <stdlib.h>'],
    ['HAVE_QSORT_R',        'qsort_r',          '#include <stdlib.h>'],
    ['HAVE_DIRFD',          'dirfd',            '#include <dirent.h>'],
    ['HAVE_FDOPENDIR',      'fdopendir',        '#include <dirent.h>'],
    ['HAVE_FLOCKFILE',      'flockfile',        '#include <stdio.h>'],
    ['HAVE_FSYNC',          'fsync',            '#include <unistd.h>'],
    ['HAVE_GETDELIM',       'getdelim',         '#include <stdio.h>'],
    ['HAVE_GETPID',         'getpid',           '#include <unistd.h>'],
    ['HAVE_GETTIMEOFDAY',   'gettimeofday',     '#include <sys/time.h>'],
    ['HAVE_INET_PTON',      'inet_pton',        '#include <arpa/inet.h>'],
    ['HAVE_LLDIV',          'lldiv',            '#include <stdlib.h>'],
    ['HAVE_MEMRCHR',        'memrchr',          '#include <string.h>'],
    ['HAVE_NRAND48',        'nrand48',          '#include <stdlib.h>'],
    ['HAVE_POLL',           'poll',             '#include <poll.h>'],
    ['HAVE_RECVMSG',        'recvmsg',          '#include <sys/socket.h>'],
    ['HAVE_REWIND',         'rewind',           '#include <stdio.h>'],
    ['HAVE_SENDMSG',        'sendmsg',          '#include <sys/socket.h>'],
    ['HAVE_SETENV',         'setenv',           '#include <stdlib.h>'],
    ['HAVE_PATHCONF',       'pathconf',         '#include <unistd.h>'],

    ['HAVE_TIMESPEC_GET',   'timespec_get',     '#include <time.h>'],
    ['HAVE_TIMEGM',         'timegm',           '#include <time.h>'],

    ['HAVE_TFIND',          'tfind',            '#include <search.h>'],
    ['HAVE_TDESTROY',       'tdestroy',         '#include <search.h>'],

    ['HAVE_STRTOF',         'strtof',           '#include <stdlib.h>'],
    ['HAVE_STRTOLL',        'strtoll',          '#include <stdlib.h>'],

    ['HAVE_STRNDUP',        'strndup',          '#include <string.h>'],
    ['HAVE_STRDUP',         'strdup',           '#include <string.h>'],
    ['HAVE_STRSEP',         'strsep',           '#include <string.h>'],
    ['HAVE_STRLCPY',        'strlcpy',          '#include <string.h>'],
    ['HAVE_STRNLEN',        'strnlen',          '#include <string.h>'],
    ['HAVE_STRNSTR',        'strnstr',          '#include <string.h>'],
    ['HAVE_STRTOK_R',       'strtok_r',         '#include <string.h>'],
    ['HAVE_STRVERSCMP',     'strverscmp',       '#include <string.h>'],
    ['HAVE_STRCASESTR',     'strcasestr',       '#include <string.h>'],

    ['HAVE_SWAB',           'swab',             '#include <unistd.h>'],

    ['HAVE_STRCASECMP',     'strcasecmp',       '#include <strings.h>'],
]

libcompat_sources = []

# Check all functions in libcompat_functions array
foreach f : libcompat_functions
    if cc.has_function(f.get(1), prefix : vlc_conf_prefix + f.get(2))
        cdata.set(f.get(0), 1)
    else
        libcompat_sources += f.get(1) + '.c'
    endif
endforeach

# These functions need to be checked with has_header_symbol
# MinGW-w64 implements those as static inline, not functions with C linkage
libcompat_functions = [
    ['HAVE_GMTIME_R',       'gmtime_r',         'time.h'],
    ['HAVE_REALPATH',       'realpath',         'stdlib.h'],
    ['HAVE_ASPRINTF',       'asprintf',         'stdio.h'],
    ['HAVE_VASPRINTF',      'vasprintf',        'stdio.h'],
    ['HAVE_LOCALTIME_R',    'localtime_r',      'time.h'],
]

foreach f : libcompat_functions
    if cc.has_header_symbol(f.get(2), f.get(1), prefix : vlc_conf_prefix)
        cdata.set(f.get(0), 1)
    else
        libcompat_sources += f.get(1) + '.c'
    endif
endforeach

# Check for function 'nanf' (optionally link with libm if it exists)
if cc.has_function('nanf', prefix: vlc_conf_prefix + '#include <math.h>', dependencies: m_lib)
    cdata.set('HAVE_NANF', 1)
endif

# Check for function 'sincos' (optionally link with libm if it exists)
if cc.has_function('sincos', prefix: vlc_conf_prefix + '#include <math.h>', dependencies: m_lib)
    cdata.set('HAVE_SINCOS', 1)
else
    libcompat_sources += 'sincos.c'
endif

# Check for function 'fdatasync' (define it to 'fsync' if missing)
if not cc.has_function('fdatasync', prefix: vlc_conf_prefix + '#include <unistd.h>')
    cdata.set('fdatasync', 'fsync')
endif

#
# Socket library checks
#

# Check for socket library
socket_libs = cc.find_library('socket', required: false)

# Check for function 'connect' (optionally link with socket lib if it exists)
if not cc.has_function('connect', prefix: vlc_conf_prefix + '#include <sys/socket.h>', dependencies: socket_libs)
    
    if host_system == 'windows'
        # If not found and on windows:
        socket_libs = []
        socket_libs += cc.find_library('iphlpapi', required: true)
        socket_libs += cc.find_library('ws2_32', required: true)
    endif
endif

#
# Additional checks
#

# Set FORTIFY_SOURCE only if no sanitizers are enabled
# TODO: Only enable if optimizations are on!
#if get_option('b_sanitize') == 'none'
#    cdata.set('_FORTIFY_SOURCE', 2)
#endif

# Check which kind of restrict keyword is supported
# Program based on autoconf c.m4
#
# Copyright (C) 2001-2012 Free Software Foundation, Inc.
#
# Written by David MacKenzie, with help from
# Akim Demaille, Paul Eggert,
# Franc,ois Pinard, Karl Berry, Richard Pixley, Ian Lance Taylor,
# Roland McGrath, Noah Friedman, david d zuhn, and many others.
restrict_test = '''
    #define restrict_kw @0@
    typedef int * int_ptr;
    int foo (int_ptr restrict_kw ip) { return ip[0]; }

    int main() {
        int s[1];
        int * restrict_kw t = s;
        t[0] = 0;
        return foo(t);
    }
'''

# Order is the same as in AC_C_RESTRICT

# Check for __restrict support
if cc.compiles(restrict_test.format('__restrict'), name: 'Test __restrict support')
    cdata.set('restrict', '__restrict')

# Check for __restrict__ support
elif cc.compiles(restrict_test.format('__restrict__'), name: 'Test __restrict__ support')
    cdata.set('restrict', '__restrict__')

# Check for _Restrict support
elif cc.compiles(restrict_test.format('_Restrict'), name: 'Test _Restrict support')
    cdata.set('restrict', '_Restrict')

# Check for restrict support
elif not cc.compiles(restrict_test.format('restrict'), name: 'Test restrict support')
    cdata.set('restrict', '')
endif

# Check for __attribute__((packed)) support
if cc.compiles('struct __attribute__((packed)) foo { int bar; };',
               name : '__attribute__((packed))')
    cdata.set('HAVE_ATTRIBUTE_PACKED', 1)
endif

# Check for C11 _Thread_local storage qualifier support
if cc.compiles('_Thread_local int foo = 0;', name: 'Test _Thread_local support')
    cdata.set('HAVE_THREAD_LOCAL', 1)
endif

# Check for max_align_t type
if cc.has_type('max_align_t', prefix: '#include <stddef.h>')
    cdata.set('HAVE_MAX_ALIGN_T', 1)
endif

# Check for struct timespec
if cc.has_type('struct timespec', prefix: '#include <time.h>')
    cdata.set('HAVE_STRUCT_TIMESPEC', 1)
endif

# Add -fvisibility=hidden if compiler supports those
add_project_arguments(
    cc.get_supported_arguments('-fvisibility=hidden'),
    language : ['c'])

# Check for struct sockaddr_storage type
# Define it to `sockaddr` if missing
have_sockaddr_storage = cc.has_type('struct sockaddr_storage', prefix: '#include <sys/socket.h>')

if not have_sockaddr_storage
    have_sockaddr_storage = cc.has_type('struct sockaddr_storage', prefix: '#include <winsock2.h>')
endif

if not have_sockaddr_storage
    cdata.set('sockaddr_storage', 'sockaddr')
endif

# Check for struct ss_family type
# Define it to `sa_family` if missing
if not cc.has_type('struct ss_family', prefix: '#include <sys/socket.h>')
    cdata.set('ss_family', 'sa_family')
endif

# Check for ssize_t type
# Define it to `int` if missing
if not cc.has_type('ssize_t', prefix: '#include <sys/types.h>')
    cdata.set('ssize_t', 'int')
endif

# Check for struct pollfd type
pollfd_prefix = '#include <sys/types.h>\n'
if cdata.get('HAVE_POLL', 0) == 1
    pollfd_prefix += '#include <poll.h>'
elif host_machine == 'windows'
    pollfd_prefix += '#include <winsock2.h>'
endif

if cc.has_type('struct pollfd', prefix: pollfd_prefix)
    cdata.set('HAVE_STRUCT_POLLFD', 1)
endif

# Check for locale_t type in C++ locale header 
if cpp.has_type('locale_t', prefix : '#include <locale>')
    cdata.set('HAVE_CXX_LOCALE_T', 1)
endif

# Check if assert.h has static_assert
if cc.has_header_symbol('assert.h', 'static_assert')
    cdata.set('HAVE_STATIC_ASSERT', 1)
endif

# Check if build machine is big endian
if build_machine.endian() == 'big'
    cdata.set('WORDS_BIGENDIAN', 1)
endif

# Define the shared library extension
if host_system == 'windows'
    cdata.set_quoted('LIBEXT', '.dll')
elif host_system == 'darwin'
    cdata.set_quoted('LIBEXT', '.dylib')
else
    cdata.set_quoted('LIBEXT', '.so')
endif

#
# Populate config.h with additional infos
#

cdata.set_quoted('VERSION', meson.project_version())
cdata.set('VERSION_MAJOR', vlc_version_major)
cdata.set('VERSION_MINOR', vlc_version_minor)
cdata.set('VERSION_REVISION', vlc_version_revision)

cdata.set_quoted('PACKAGE_VERSION', meson.project_version())
cdata.set_quoted('VERSION_MESSAGE', meson.project_version() + ' ' + vlc_version_codename)
cdata.set_quoted('PACKAGE', meson.project_name())
cdata.set_quoted('PACKAGE_NAME', meson.project_name())
cdata.set_quoted('PACKAGE_STRING', meson.project_name())
cdata.set_quoted('COPYRIGHT_YEARS', vlc_copyright_years)
cdata.set_quoted('COPYRIGHT_MESSAGE', 'Copyright © ' + vlc_copyright_years + ' the VideoLAN team')



cdata.set_quoted('VLC_COMPILER', cc.get_id() + ' ' + cc.version())

# Paths
cdata.set_quoted('LIBDIR', get_option('libdir'))
cdata.set_quoted('LIBEXECDIR', get_option('libexecdir'))
cdata.set_quoted('PKGDATADIR', join_paths(get_option('datadir'), meson.project_name()))
cdata.set_quoted('PKGLIBDIR', join_paths(get_option('libdir'), meson.project_name()))
cdata.set_quoted('PKGLIBEXECDIR', join_paths(get_option('libexecdir'), meson.project_name()))
cdata.set_quoted('SYSDATADIR', get_option('datadir'))
cdata.set_quoted('LOCALEDIR', get_option('localedir'))

# TODO:
cdata.set_quoted('VLC_COMPILE_BY', 'A Cone')
cdata.set_quoted('VLC_COMPILE_HOST', 'x86_64-apple-darwin17.5.0')

# Enable stream outputs
if get_option('stream_outputs')
    cdata.set('ENABLE_SOUT', 1)
endif

# Enable VLM
if get_option('videolan_manager')
    if not get_option('stream_outputs')
        error('The VideoLAN manager requires stream outputs.')
    endif
    cdata.set('ENABLE_VLM', 1)
endif

# Enable add-on manager modules
if get_option('addon_manager')
    cdata.set('ENABLE_ADDONMANAGERMODULES', 1)
endif

# Allow running as root
# (useful for people running on embedded platforms)
if get_option('run_as_root')
    cdata.set('ALLOW_RUN_AS_ROOT', 1)
endif

# Optimize for memory usage vs speed
if get_option('optimize_memory')
    cdata.set('OPTIMIZE_MEMORY', 1)
endif

# Allow binary package maintainer to pass a custom string
# to avoid cache problem
if get_option('binary_version') != ''
    cdata.set_quoted('DISTRO_VERSION', get_option('binary_version'))
endif


# Font options
if get_option('default_font_path') != ''
    cdata.set_quoted('DEFAULT_FONT_FILE', get_option('default_font_path'))
endif

if get_option('default_monospace_font_path') != ''
    cdata.set_quoted('DEFAULT_MONOSPACE_FONT_FILE', get_option('default_monospace_font_path'))
endif

if get_option('default_font_family') != ''
    cdata.set_quoted('DEFAULT_FAMILY', get_option('default_font_family'))
endif

if get_option('default_monospace_font_family') != ''
    cdata.set_quoted('DEFAULT_MONOSPACE_FAMILY', get_option('default_monospace_font_family'))
endif

# Generate config.h
configure_file(input : 'config.h.meson',
  output : 'config.h',
  configuration : cdata)

# Some missing functions are implemented in compat
subdir('compat')

# libvlccore
subdir('src')

# libvlc
subdir('lib')

# vlc
subdir('bin')

# modules
subdir('modules')
