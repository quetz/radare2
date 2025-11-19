project('mimalloc', 'c',
  version: '3.1.5',
  meson_version: '>=0.55.0',
)

cc = meson.get_compiler('c')

mi_sources = [
  'src/alloc.c',
  'src/alloc-aligned.c',
  'src/alloc-posix.c',
  'src/arena.c',
  'src/arena-meta.c',
  'src/bitmap.c',
  'src/heap.c',
  'src/init.c',
  'src/libc.c',
  'src/options.c',
  'src/os.c',
  'src/page.c',
  'src/page-map.c',
  'src/random.c',
  'src/stats.c',
  'src/prim/prim.c',
  'src/prim/osx/alloc-override-zone.c',
]

mi_c_args = [
  '-DMI_STATIC_LIB',
  '-DMI_MALLOC_OVERRIDE',
]

# Add option-based defines
if get_option('mi_secure')
  mi_c_args += '-DMI_SECURE=4'
endif

if get_option('mi_debug_full')
  mi_c_args += '-DMI_DEBUG=3'
endif

if get_option('mi_padding')
  mi_c_args += '-DMI_PADDING=1'
endif

if get_option('mi_guarded')
  mi_c_args += '-DMI_GUARDED=1'
endif

if get_option('mi_show_errors')
  mi_c_args += '-DMI_SHOW_ERRORS=1'
endif

if get_option('mi_xmalloc')
  mi_c_args += '-DMI_XMALLOC=1'
endif

if get_option('mi_opt_arch')
  if host_machine.cpu_family() == 'x86_64'
    mi_c_args += ['-march=haswell', '-mavx2']
  elif host_machine.cpu_family() == 'aarch64'
    mi_c_args += '-march=armv8.1-a'
  endif
endif

if get_option('mi_opt_simd')
  mi_c_args += '-DMI_USE_SIMD=1'
endif

if get_option('mi_osx_interpose')
  mi_c_args += '-DMI_OSX_INTERPOSE=1'
endif

if get_option('mi_osx_zone')
  mi_c_args += '-DMI_OSX_ZONE=1'
endif

if get_option('mi_local_dynamic_tls')
  mi_c_args += '-DMI_TLS_PTHREADS=1'
endif

if get_option('mi_track_valgrind')
  mi_c_args += '-DMI_TRACK_VALGRIND=1'
endif

if get_option('mi_track_asan')
  mi_c_args += '-DMI_TRACK_ASAN=1'
endif

if get_option('mi_skip_collect_on_exit')
  mi_c_args += '-DMI_SKIP_COLLECT_ON_EXIT=1'
endif

if get_option('mi_no_thp')
  mi_c_args += '-DMI_USE_THP=0'
endif

mi_includes = [
  include_directories('include'),
]

mi_deps = []
if host_machine.system() != 'windows'
  # pthread for Unix-like systems
  mi_deps += dependency('threads')
endif

libmimalloc = static_library('mimalloc',
  mi_sources,
  c_args: mi_c_args,
  include_directories: mi_includes,
  dependencies: mi_deps,
  implicit_include_directories: false,
)

mimalloc_dep = declare_dependency(
  link_whole: libmimalloc,
)
