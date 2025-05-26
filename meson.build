# SPDX-License-Identifier: BSL-1.0

project('editor', ['c', 'cpp'], default_options : ['buildtype=debugoptimized'])

if meson.get_compiler('cpp').get_id() == 'gcc'
    add_project_arguments('-fvisibility=hidden', '-fvisibility-inlines-hidden', language: 'cpp')
endif

add_project_arguments('-std=gnu17', language : 'c')
add_project_arguments('-Wall', '-Wextra', language: 'c')
add_project_arguments('-Werror=return-type', '-Werror=implicit-function-declaration', language: 'c')

add_project_arguments('-std=gnu++17', language : 'cpp')
add_project_arguments('-Wall', '-Wextra', language: 'cpp')
add_project_arguments('-Werror=return-type', language: 'cpp')

qt_version = get_option('qt')
qt = import(qt_version)
qt_dep = dependency(qt_version, modules: ['Core', 'Concurrent'])

syntax_dep = []
syntax_qrc = qt.compile_resources(sources: 'syntax.qrc')

if qt_version == 'qt5'
    tuiwidgets_dep = dependency('TuiWidgets', version: '>= 0.2.1')
    if tuiwidgets_dep.version().version_compare('>= 0.2.2')
      add_project_arguments('-DHAS_TUIWIDGETS_0_2_2', language: 'cpp')
    endif
    posixsignalmanager_dep = dependency('PosixSignalManager')
    if get_option('syntax_highlighting')
        syntax_dep = declare_dependency(dependencies:dependency('KF5SyntaxHighlighting'), compile_args: ['-DSYNTAX_HIGHLIGHTING'], sources: syntax_qrc)
    endif
else
    tuiwidgets_dep = dependency('TuiWidgetsQt6', version: '>= 0.2.1')
    if tuiwidgets_dep.version().version_compare('>= 0.2.2')
      add_project_arguments('-DHAS_TUIWIDGETS_0_2_2', language: 'cpp')
    endif
    posixsignalmanager_dep = dependency('PosixSignalManagerQt6')
    if get_option('syntax_highlighting')
        syntax_dep = declare_dependency(dependencies:dependency('KF6SyntaxHighlighting'), compile_args: ['-DSYNTAX_HIGHLIGHTING'], sources: syntax_qrc)
    endif
endif

install_man('manpages/chr.1')
install_man('manpages/chr.de.1', locale: 'de')

subdir('src')
