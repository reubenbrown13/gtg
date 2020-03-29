project('gtg',
          version: '0.4.0',
    meson_version: '>= 0.50.0',
  default_options: ['warning_level=2'],
)

# Importing modules
gnome = import('gnome')
i18n  = import('i18n')
python = import('python3')

message('Looking for dependencies')
python_bin = python.find_python()
if not python_bin.found()
    error('No valid python3 binary found')
else
    message('Found python3 binary')
endif



# Constants
PACKAGE_URL = 'https://github.com/getting-things-gnome/gtg'
PACKAGE_URL_BUG = 'https://github.com/getting-things-gnome/gtg/issues'
PROJECT_RDNN_NAME = 'org.gnome.GTG'


# NAME_SUFFIX is used in the about dialog
if get_option('profile') == 'development'
  PROFILE = '.Devel'
  NAME_SUFFIX = ' (Development Snapshot)'
else
  PROFILE = ''
  NAME_SUFFIX = ''
endif


APPLICATION_ID = 'org.gnome.GTG@0@'.format(PROFILE)

# Dependencies
dependency('pygobject-3.0', version: '>= 3.29.1')

env = environment()

python_dir = join_paths(get_option('prefix'), python.sysconfig_path('purelib'))
LIBEXEC_DIR = join_paths(get_option('prefix'), get_option('libexecdir'))
EXTENSION_DIR = join_paths(get_option('prefix'), get_option('libdir'), meson.project_name())
DATA_DIR = join_paths(get_option('prefix'), get_option('datadir'), meson.project_name())
bindir = join_paths(get_option('prefix'), get_option('bindir'))

# Profiles
if get_option('profile') == 'development'
    profile = 'Devel'
    name_suffix = ' (Development)'
else
    profile = ''
    name_suffix = ''
endif


conf = configuration_data()
conf.set('PACKAGE_URL', PACKAGE_URL)
conf.set('DATA_DIR', DATA_DIR)
conf.set('EXTENSION_DIR', EXTENSION_DIR)
conf.set('LOCALE_DIR', join_paths(get_option('prefix'), get_option('datadir'), 'locale'))
conf.set('PYTHON_DIR', python_dir)
conf.set('PYTHON_EXEC_DIR', join_paths(get_option('prefix'), python.sysconfig_path('stdlib')))
conf.set('PYTHON', python_bin.path())
conf.set('libexecdir', LIBEXEC_DIR)


if get_option('profile') == 'development'
    conf.set('VERSION', ''.join([run_command(['git', 'describe', '--long', '--tags']).stdout(), get_option('profile')]))
else
    conf.set('VERSION', meson.project_version())
endif

conf.set_quoted('PROFILE', profile)

if get_option('profile') == 'development'
  PROJECT_RDNN_NAME = 'org.Gnome.GTGDevel'
endif

conf.set('APPID', PROJECT_RDNN_NAME)


install_subdir(
    'GTG',
    install_dir: bindir,
)

meson.add_install_script('meson_post_install.py')


