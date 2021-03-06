project('zlib', 'c', version : '1.2.11', license : 'zlib')

cc = meson.get_compiler('c')

link_args = []
compile_args = []
if cc.get_id() == 'msvc'
  add_project_arguments('-D_CRT_SECURE_NO_DEPRECATE',
    '-D_CRT_NONSTDC_NO_DEPRECATE', language : 'c')
else
  # Don't spam consumers of this wrap with these warnings
  compile_args += cc.get_supported_arguments(['-Wno-implicit-fallthrough',
                                              '-Wno-implicit-function-declaration'])
  if cc.get_id() == 'gcc' and host_machine.system() != 'windows'
    vflag = '-Wl,--version-script,@0@/zlib.map'.format(meson.current_source_dir())
    link_args += [vflag]
  endif
endif

if cc.get_id() == 'msvc'
  link_args += '/MACHINE:X64'
endif

src = files([
  'adler32.c',
  'crc32.c',
  'deflate.c',
  'infback.c',
  'inffast.c',
  'inflate.c',
  'inftrees.c',
  'trees.c',
  'zutil.c',
  'compress.c',
  'uncompr.c',
  'gzclose.c',
  'gzlib.c',
  'gzread.c',
  'gzwrite.c'])

headers = files(['zconf.h', 'zlib.h'])

if host_machine.system() == 'windows'
  win = import('windows')
  win_args = []
  if cc.get_id() != 'msvc'
    win_args += '-DGCC_WINDRES'
  endif
  src += win.compile_resources('win32/zlib1.rc', args : win_args)
endif

zlib = static_library('z', src,
  c_args : compile_args,
  link_args : link_args,
  install : true)

incdir = include_directories('.')

zlib_dep = declare_dependency(
  link_with : zlib,
  include_directories : incdir)

install_headers(headers)

pkg = import('pkgconfig')

pkg.generate(zlib,
             name: 'zlib',
             description: 'zlib compression library')
