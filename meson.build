project('lua', 'c', version : '5.3.0', license : 'mit')

cc = meson.get_compiler('c')

system_deps = [cc.find_library('dl', required : false),
  cc.find_library('m', required : false)
]

add_project_arguments('-DLUA_COMPAT_5_1 -DLUA_COMPAT_5_2', language : 'c')

if host_machine.system() == 'linux'
  add_project_arguments('-DLUA_USE_LINUX', language : 'c')
  readline_dep = cc.find_library('readline')
elif host_machine.system() == 'darwin'
  add_project_arguments('-DLUA_USE_MACOSX', language : 'c')
  readline_dep = cc.find_library('readline')
elif host_machine.system() == 'windows'
  readline_dep = []
else
  readline_dep = []
  error('Unsupported platform')
endif

src = [
  'lapi.c',
  'lcode.c',
  'lctype.c',
  'ldebug.c',
  'ldo.c',
  'ldump.c',
  'lfunc.c',
  'lgc.c',
  'llex.c',
  'lmem.c',
  'lobject.c',
  'lopcodes.c',
  'lparser.c',
  'lstate.c',
  'lstring.c',
  'ltable.c',
  'ltm.c',
  'lundump.c',
  'lvm.c',
  'lzio.c',
  'lauxlib.c',
  'lbaselib.c',
  'lbitlib.c',
  'lcorolib.c',
  'ldblib.c',
  'liolib.c',
  'lmathlib.c',
  'loslib.c',
  'lstrlib.c',
  'ltablib.c',
  'lutf8lib.c',
  'loadlib.c',
  'linit.c',
]

if get_option('default_library') == 'shared'
  add_project_arguments('-DLUA_BUILD_AS_DLL', language : 'c')
endif

lualib = library('lua', src,
  dependencies : system_deps)

incdir = include_directories('.')

defs = []
if get_option('default_library') == 'shared'
  defs += '-DLUA_BUILD_AS_DLL'
endif

lua_dep = declare_dependency(link_with : lualib,
  include_directories : incdir, compile_args: defs)

executable('luai', 'lua.c',
  link_with : lualib,
  dependencies : readline_dep)

# This only works with static linking.
if get_option('default_library') == 'static'
  executable('luac', 'luac.c',
    link_with : lualib)
endif
