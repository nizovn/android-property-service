# @@@LICENSE
#
# Copyright (c) 2013 Simon Busch <morphis@gravedo.de>
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# LICENSE@@@

cmake_minimum_required(VERSION 2.8.7)

project(android-property-service C)

include(webOS/webOS)
webos_modules_init(1 0 0 QUALIFIER RC2)
webos_component(0 1 0)

include(FindPkgConfig)

pkg_check_modules(GLIB2 REQUIRED glib-2.0)
include_directories(${GLIB2_INCLUDE_DIRS})
webos_add_compiler_flags(ALL ${GLIB2_CFLAGS})

pkg_check_modules(GIO2 REQUIRED gio-2.0)
include_directories(${GIO2_INCLUDE_DIRS})
webos_add_compiler_flags(ALL ${GIO2_CFLAGS})

pkg_check_modules(GIO-UNIX REQUIRED gio-unix-2.0)
include_directories(${GIO-UNIX_INCLUDE_DIRS})
webos_add_compiler_flags(ALL ${GIO-UNIX_CFLAGS_OTHER})

pkg_check_modules(LUNASERVICE2 REQUIRED luna-service2)
include_directories(${LUNASERVICE2_INCLUDE_DIRS})
webos_add_compiler_flags(ALL ${LUNASERVICE2_CFLAGS})

pkg_check_modules(PBNJSON_C REQUIRED pbnjson_c)
include_directories(${PBNJSON_C_INCLUDE_DIRS})
webos_add_compiler_flags(ALL ${PBNJSON_C_CFLAGS_OTHER})

pkg_check_modules(LUNAPREFS REQUIRED luna-prefs)
include_directories(${LUNAPREFS_INCLUDE_DIRS})
webos_add_compiler_flags(ALL ${LUNAPREFS_CFLAGS_OTHER})

file(GLOB SOURCE_FILES src/main.c src/property_service.c src/luna_service_utils.c)

webos_add_compiler_flags(ALL -Wall)
webos_add_linker_options(ALL --no-undefined)

add_executable(android-property-service ${SOURCE_FILES})
target_link_libraries(android-property-service
    ${GLIB2_LDFLAGS} ${LUNASERVICE2_LDFLAGS} ${PBNJSON_C_LDFLAGS}
    ${GIO2_LDFLAGS} ${GIO-UNIX_LDFLAGS} ${GOBJECT2_LDFLAGS} rt pthread
    android-properties)

webos_build_daemon()
webos_build_system_bus_files()
