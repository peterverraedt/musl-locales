project(locales C)
cmake_minimum_required(VERSION 2.8)
include(GNUInstallDirs)
list (APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)
INCLUDE_DIRECTORIES(${CMAKE_BINARY_DIR})
set(GETTEXT_PACKAGE "musl-locales")
set(MUSL_LOCPATH i18n/locales/musl CACHE PATH "Locales directory" FORCE)
configure_file(config.h.in config.h)
configure_file(locale.in 00locale.sh)
find_package(Intl)
set(LOCALE_SOURCES locale.c categories.c categories.h config.h)
add_executable(locale ${LOCALE_SOURCES})
if(Intl_LIBRARIES)
    target_link_libraries(locale ${Intl_LIBRARIES})
endif()
target_compile_options(locale
  PRIVATE $<$<COMPILE_LANGUAGE:C>:-std=gnu11 -Wno-pedantic>
)
install(PROGRAMS ${CMAKE_BINARY_DIR}/00locale.sh DESTINATION 
"${CMAKE_INSTALL_FULL_SYSCONFDIR}/profile.d")
install(TARGETS locale DESTINATION bin)
add_subdirectory(po)
add_subdirectory(musl-po)
